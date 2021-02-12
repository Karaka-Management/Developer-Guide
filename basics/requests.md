# Requests

Requests can be either incoming requests such as http requests on the server side or outgoing requests such as ajax requests on the client side. At the same time it's also possible to generate outgoing requests on the server side for microservices as well as REST requests to third party APIs.

## Http Requests

A http request in its' most basic form only consists of a uri.

```php
$request = new HttpRequest(new HttpUri('http://your_request.com/url?to=call'));
```

### Localization

The request localization can be defined witht he second parameter

```php
$request = new HttpRequest(new HttpUri('http://your_request.com/url?to=call'), new Localization());
```

In all web applications (e.g. backend, api, etc.) the localization and uri are automatically propagated during the initialization of the application.

## Rest Requests

With the rest implementation it's possible to make http requests and read the response.

```php
$request = new HttpRequest(new HttpUri('http://your_request.com/url?to=call'));
$result  = Rest::Httprequest($request);
```

Alternatively it's also possible to make a rest request directly from the Http request object.

```php
$request = new HttpRequest(new HttpUri('http://your_request.com/url?to=call'));
$result  = $request->rest();
```

The result contains the raw response text. By defining the request method it's also possible to make other requests such as `PUT` requests.

```php
$request = new HttpRequest(new HttpUri('http://your_request.com/url?to=call'));
$request->setMethod(RequestMethod::PUT);
$result = Rest::request($request);
```

## Socket Requests

## Websocket Requests

## JavaScript Requests
