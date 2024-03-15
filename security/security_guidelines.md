# Security Guidelines

## CSRF

The tool to protect clients from CSRF is a randomly generated CSRF token, that can be used inside the URI generator. It's highly recommended to make use of this token whenever possible to reduce the risk of CSRF attacks.

Example usage:

```html
<form action="<?= UriFactory::build('{/api}path?csrf={$CSRF}')" ...>
    ...
</form>
```

Now the application will receive the automatically generated CSRF token as query parameter for further use. If the CSRF token is not the same as the one associated with the client on the server side the client will receive a 403 HTTP response. The CSRF however doesn't have be specified, if that's the case **every module itself must make sure whether a valid CSRF token is required** or not. The reason for this is that third party requests are a possibility as well, and sharing the private CSRF token would render it useless.

Since the validation of the CSRF token is performed automatically it is only necessary to check the existence, since if it exists it has to be valid.

Example usage in a module handling a API request:

```php
if($request->getData('CSRF') === null) {
    $response->setStatusCode(RequestStatus::R_403);

    /* optional */
    $response->set($request->uri->__toString(), new Notify('Unknown referrer!', NotifyType::INFO));

    return;
}
```

### When do I check for the CSRF token validity/existence?

Always! Except the request has the potential to come from third party referrers. Here a few examples of requests that must always have a valid CSRF token:

1. Login/logout
2. Creating/updating/deleting something
3. Uploading media files
4. Changes in user settings

Here some examples of requests that **MAY** not need a validation (mostly API GET requests):

1. Get news posts
2. Get last log message

It's important to understand that the CSRF token is not equivalent with authentication or API token. Clients can be logged out and still need a CSRF token and obviously vice versa.

## Headers

The following headers must be set for every web application. By default they are already set in the `WebApplication` which gets expanded by all other web applications.

### Content-Security-Policy

Scripts and frames must be provided by the own server. This is important in order to prevent the injection of other scripts and clickjacking. Inline javascript is prohibited and may only be defined in the application and not in any modules.

The default CSP looks like the following:

```php
$scriptSrc = \bin2hex(\random_bytes(32));
$this->app->appSettings->setOption('script-nonce', $scriptSrc);
$response->header->set('content-security-policy',
    'base-uri \'self\';'
    . 'object-src \'none\';'
    . 'script-src \'nonce-' . $scriptSrc . '\' \'strict-dynamic\';'
    . 'worker-src \'self\';'
);
```

Javascript can now be included like this:

```php
$head  = $response->data['Content']->head;
$nonce = $this->app->appSettings->getOption('script-nonce');

$head->addAsset(AssetType::JSLATE, 'Resources/chartjs/Chartjs/chart.js?v=' . $this->app->version, ['nonce' => $nonce]);
$head->addAsset(AssetType::JSLATE, 'Modules/ItemManagement/Controller.js?v=' . self::VERSION, ['nonce' => $nonce, 'type' => 'module']);
```

### X-XSS-Protection

This header tells the client browser to use local xss protection if available.

```php
$response->header->set('x-xss-protection', '1; mode=block');
```

### X-Content-Type-Options

By using this header browsers which support this feature will ignore the content/mime and recognize the file by the provided header only.

```php
$response->header->set('x-content-type-options', 'nosniff');
```

### X-Frame-Options

The x-frame-options is providing the same protection for frames as the content-security-policy header. Please only use this header in addition to the content-security-policy if you have to but make sure the rules don't contradict with the content-security-policy.

```php
$response->header->set('x-frame-options', 'SAMEORIGIN');
```

## Superglobals

Superglobals are not available throughout the application and the values can only be accessed through middleware classes like:

* SessionManager
* CookieJar
* Request
* Response

In some cases superglobals will even be overwritten by values from these classes before generating a response. Do not directly access superglobals!

## Input validation

Input validation can be implemented on multiple levels.

1. Regex validation in html/javascript by using the `pattern=""` attribute
2. Type hints for method parameters wherever possible.
3. Making use of the `Validation` classes as much as possible
4. **Do not** sanitize! Accept or dismiss!

## Inclusion and file paths

Be vigilant of where and how the path for the following scenarios comes from:

1. `include $path;`
2. `\fopen($path);`
3. `\file_get_contents('../relative/path/to/' . $path);`
4. `\mkdir($path);`

These are just a few examples but it is very important to make sure, that these paths only have access to wherever the programmer intended them for. If a file path is provided or influenced by a user you can make sure that it is within a trusted subdirectory:

```php
if (!Guard::isSafePath($outputDir, __DIR__ . '/../../../')) {
    return;
}
```

Using this guard ensures that a file path is within the provided path.
