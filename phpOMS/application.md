# Application Sample

The following application is a minimal sample in order to show how it's possible to set up a standalone application with the `phpOMS` framework. Please note that for simplicity purposes many framework features, docblocks and architectural aspects are omitted.

## .htaccess

The .htaccess file can be used to enable URL rewriting, file compression for css, js files, caching, and much more. In this sample application we will only do the bare minimum by enabling URL rewriting.

```txt
# .htaccess
# enable URL rewriting
<ifmodule mod_rewrite.c>
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(.*)$ /?{QUERY_STRING} [QSA]
    RewriteCond %{HTTPS} !on
    RewriteCond %{HTTP_HOST} !^(127\.0\.0)|(192\.)|(172\.)
    RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}
</ifmodule>
```

## index.php

In the index file the application gets initialized and executed. 

```php
<?php 
declare(strict_types=1);

// index.php
// initialize application and output response (entry point for the app)

\ob_start();
require_once __DIR__ . '/phpOMS/Autoloader.php';
$config = require_once __DIR__ . '/config.php';

$App = new \app\Application($config);
echo $App->run(); // outputs the application response

\ob_end_flush();
```

We use output buffering `\ob_start()` and `\ob_end_flush()` which allows the application to internally modify the response before it gets returned to the user. 

## Application.php

The application file is responsible for initializing the application resources, handling the request and response population (see Router and Dispatcher) as well as rendering the main view. Another task which is often performed in this file is the user authentication.

```php
<?php 
declare(strict_types=1);

// app/Application.php
// Application where the framework components are initialized

namespace app;

use phpOMS\Application\ApplicationAbstract; /* provides many member variables which are often shared with controllers */
use phpOMS\Dispatcher\Dispatcher;
use phpOMS\Message\Http\HttpRequest;
use phpOMS\Message\Http\HttpResponse;
use phpOMS\Router\WebRouter;
use phpOMS\Uri\UriFactory;
use phpOMS\Views\View;

class Application extends ApplicationAbstract
{
    /* initialize most framework components */
    public function __construct(array $config)
    {
        $this->router = new WebRouter();

        // sample routes for different endpoints are defined in a routes file
        $this->router->importFromFile(__DIR__ . '/Routes.php');

        $this->dispatcher = new Dispatcher($this);

        // usually much more stuff happens here for real world apps (e.g. DB initialization, session, ...)
    }

    /* setup response and return it */
    public function run() : string
    {
        $request  = $this->initRequest();
        $response = $this->initResponse($request);

        $pageView = $this->initMainPageTemplate($request, $response);

        // bind the page view to the response object. This is rendered later on.
        $response->set('Content', $pageView);

        /* get data from URL endpoints defined by the routes */
        $dispatch = $this->dispatcher->dispatch(
            $this->router->route(
                $request->uri->getRoute(),
                $request->getDataString('CSRF'), // optional: only required if csrf tokens are used otherwise use null
                $request->getRouteVerb() // e.g. get, post, put ...
            ),
            $request, // this is passed to the endpoint
            $response // this is passed to the endpoint
            // define more data you want to pass to the endpoints if necessary
        );

        // bind data (in this case the dispatcher response) to the view
        $pageView->addData('dispatch', $dispatch);

        // push the headers (no changes to the header are possible afterwards)
        $response->header->push();

        // renders the content of the response object (depends on the content type, text/html, json, ...)
        return $response->getBody();
    }

    /* initialize request of the user */
    private function initRequest() : Request
    {
        // initialize request from the superglobals which are automatically populated
        $request = Request::createFromSuperglobals();

        // optional: will transform the URL path and sub-paths to hashs
        $request->createRequestHashs(0);

        // if your application is located in a web-subfolder for easier handling
        $request->uri->setRootPath('/');

        // this will allow you to create urls based on request data
        UriFactory::setupUriBuilder($request->uri);

        return $request;
    }

    /* initialize response object */
    private function initResponse(HttpRequest $request) : HttpResponse
    {
        $response = new HttpResponse();

        // you could use the request content-type in order to define the response content-type
        $response->header->set('content-type', 'text/html; charset=utf-8');

        $response->header->set('x-xss-protection', '1; mode=block');
        // more CSP can be defined here

        return $response;
    }

    /* initialize main page template. if many pages have a different page template you could put this into the corresponding controllers. */
    private function initMainPageTemplate(HttpRequest $request, HttpResponse $response) : View
    {
        $pageView = new View(null, $request, $response);
        $pageView->setTemplate('/app/tpl/index'); // templates need file ending .tpl.php
        $pageView->setData('title', 'Default Title!'); // can be changed later on

        return $pageView;
    }
}
```

## Routes.php

The routes file contains the routing information. which is responsible for matching URLs to application end-points.

```php
<?php 
declare(strict_types=1);

// app/Routes.php
// Routes for the application

use phpOMS\Router\RouteVerb;

return [
    '^/*$' => [ // root path has two endpoints defined for get requests
        [
            'dest' => '\app\controller\TestController:home', // method
            'verb' => RouteVerb::GET,
        ],
        [
            'dest' => '\app\controller\TestController::exit', // static function
            'verb' => RouteVerb::GET,
        ],
    ],
    '^/otherUri$' => [ // has one endpoint defined for a get request and one for a set request
        [
            'dest' => '\app\controller\TestController::exit',
            'verb' => RouteVerb::GET,
        ],
        [
            'dest' => '\app\controller\TestController::thisisset', // actually not implemented
            'verb' => RouteVerb::SET,
        ],
    ],
    '^/overwritten.*$' => [ // has one endpoint defined for a get request
        [
            'dest' => '\app\controller\TestController:newMainTemplate',
            'verb' => RouteVerb::GET,
        ],
    ],
];
```

## index.tpl.php

The index template is the main template which is used to render all frontend responses from the dispatcher to the user. The layout is completely in the hands of the developer and designer.

```php
<!-- app/tpl/index.tpl.php Main template -->
<html>
    <head>
        <title><?= $this->getDataString('title') ?? ''; ?></title>
    </head>
    <body>
        <div>This is the main template....</div>
        <div>What follows next comes from the dispatcher results:</div>
        <main>
        <?php
            $dispatch = $this->getDataArray('dispatch') ?? []; // get data bound to the view with the key "dispatch"
            foreach($dispatch as $view) echo $view->render(); // in this case it has a 'render()' method which is called
        ?>
        </main>
    </body>
</html>
```

The `$dispatch` date is populated in the `Application.php` an can be used in this template. Often is is just one element but in some cases multiple endpoints get matched to a URL and provide content for the user.

## Controller.php

A controller file defines the end-point functionality. In this file the response content is generated incl. the date collection for the response. Usually there are two types of controllers, one is a UI controller which is responsible for generating UI content and the other is a API controller which is used to handle API logic (e.g. handling form data such as creating/updating new models).

```php
<?php
declare(strict_types=1);

// app/controller/TestController.php
// Sample (UI) controller which is referenced in the routes

namespace app\controller;

use phpOMS\Application\ApplicationAbstract;
use phpOMS\Message\RequestAbstract;
use phpOMS\Message\ResponseAbstract;
use phpOMS\Views\View;

use app\view\TestView;

class TestController
{
    private ApplicationAbstract $app;

    /* the dispatcher passes the ApplicationAbstract reference to the controller */
    public function __construct(ApplicationAbstract $app)
    {
        $this->app = $app;
    }

    /* Sample route endpoint (see routes) */
    public function home(RequestAbstract $request, ResponseAbstract $response, $data = null)
    {
        // special view which extends the normal `View` where we can define additional view logic
        $view = new TestView();
        $view->setTemplate('/app/tpl/welcome');

        return $view;
    }

    /* Sample route endpoint (see routes) */
    public static function exit(RequestAbstract $request, ResponseAbstract $response, $data = null)
    {
        // overwrite/set the page title used in the main template `index.tpl.php`
        $response->get('Content')->setData('title', 'New Title!!!');

        $view = new View();
        $view->setTemplate('/app/tpl/goodbye');

        return $view;
    }

    /* Sample route endpoint (see routes) */
    public function newMainTemplate(RequestAbstract $request, ResponseAbstract $response, $data = null)
    {
        $view = new View();
        $view->setTemplate('/app/tpl/overwritten');

        // overwrite the main template with a different template
        // this is one example how to define a different main template for a different a page
        $response->set('Content', $view);
    }
}
```

## View.php

A view is responsible for handling and rendering template data. Some business logic doesn't belong in the controller but rather to a specific view (i.e. how to render a user name).

```php
<?php
declare(strict_types=1);

// app/view/TestView.php
// Sample view which can hold additional view logic which can be used by the template

namespace app\view;

use phpOMS\Views\View;

class TestView extends View
{
    /* simple example of some view logic */
    public function renderBold(string $bold) : string
    {
        return '<strong>' . $bold . '</strong>';
    }
}
```

## Templates

Templates contain the actual content which should get shown/returned to the user. We already learned about the `index.tpl.php`. These template files are returned from the controller as part of a view and then rendered in the `index.tpl.php` file. Inside of a template you can access the view data and view logic and helper functions defined in the view.

```html
<!-- app/tpl/welcome.tpl.php This is shown in the index.tpl.php -->
<div><?= 'Hello ' . $this->renderBold('beautiful') . ' World!' ?></div>
```

```html
<!-- app/tpl/goodbye.tpl.php This is shown in the index.tpl.php -->
<div><?= 'Goodbye World!' ?></div>
```

```html
<!-- app/tpl/overwritten.tpl.php This becomes the new main template -->
<div>The main template got overwritten!</div>
```

