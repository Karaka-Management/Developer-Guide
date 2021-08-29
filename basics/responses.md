# Responses

Responses can be either outgoing responses such as http or socket responses on the server side or responses generated on the client side for socket/websocket requests.

## Http Response

A response can be defined with the `set()` function and the body of the response can be generated/rendered with the `getBody()` function.

```php
$response->set('response_id', {stringifiable_element});
```

The `response_id` is a unique internal id which allows you to create multiple response elements. All response elements will get combined (usually concatenated) during the rendering process.

The `{stringifiable_element}` accepts all supported types from the `StringUtils::stringify` function. e.g.

* \JsonSerializable
* array
* \Serializable
* string
* int
* float
* bool
* null
* \DateTimeInterface
* RenderableInterface
* \__toString()

All other types will be rendered as null during `getBody()`.

### Localization

The response localization can be defined witht he second parameter

```php
$response = new HttpResponse(new Localization());
```

In all web applications (e.g. backend, api, etc.) the localization is automatically propagated during the initialization of the application. This means all templates, views and controller endpoints may use the localization in order to generate localized responses.

### Default Headers

The default headers for all web applications are:

* 'content-type' => 'text/html; charset=utf-8'
* 'x-xss-protection' => '1; mode=block'
* 'x-content-type-options' => 'nosniff'
* 'x-frame-options' => 'SAMEORIGIN'
* 'referrer-policy' => 'same-origin'

In order to change the response type the header `content-type` must be changed.

```php
$response->header->set('content-type', '....', true);
```

The boolean flag at the end forces an overwrite of the previous response type. If this flag is not set the response type will not be overwritten. The reason for this flag is to protect header elements from accidentally getting overwritten.

### Sending Headers

> After the headers are sent it is not possible to change the headers or re-send them, the header is locked!. Usually you don't have to worry about this since the headers are automatically sent just before the content gets rendered.

### Text/Html Response

In most cases the text/html response is created based on a `View` which has a template assigned which then gets rendered during the response rendering process. The `View` implements `RenderableInterface` (see above). In case of frontend web applications only a view needs to be created in the controller endpoints which is then returned from the controller endpoint.

```php
public function controllerEndpoint(HttpRequest $request, HttpResponse $response, array $data = []) : RenderableInterface
{
	$view = new View($this->app->l11nManager, $request, $response);
	$view->setTemplate('....');
	$view->setData('data_id', ....);

	return $view;
}
```

### JSON Response

A json response can be created by defining the correct header and adding `\JsonSerializable` elements to the response.

```php
$response->header->set('Content-Type', MimeType::M_JSON . '; charset=utf-8', true);
$response->set('response_id' {array or \JsonSerializable};
```

### Other

Other response types besides the above described ones are all handled in the same way. You must set the response type/header and provide an element which can be converted by `StringUtils::stringify()`.

## Socket Response

## Websocket Response
