# Dispatching

The dispatching is the follow up on the routing. In the dispatcher the route destination gets resolved and executed. Dispatching can be performed on module instance methods, static functions and anonymous functions.

The return of the `dispatch()` call is an array of all end point returns.

## Basic

The dispatcher accepts a string representation of the method or static function which should be dispatched, a closure which should be executed or an array of the just mentioned options.

The `dispatch()` function accepts additionally a variable amount of parameters which will be passed to the routed method/function.

### String

#### Module

The module function can be called by providing the namespace followed by the function name concatonated with a colon `:` between the function name and the namespace.

```php
$dispatcher->dispatch('\My\Namespace:methodToCall', $methodToCallPara1, $methodToCallPara2, ...);
```

In order to allow the dispatcher to automatically create a a controller instance. The class constructore for the controller `\my\Namespace` needs to extend `ModuleAbstract` and have the format:

```php
public function __construct(ApplicationAbstract $app) {}
```

Alternatively you can also add a controller manually to the disptacher. In this case you may construct the controller as you see fit. However, the controller must extend `ModuleAbstract`.

```php
$testController = new MyController();
$dispatcher->set($testController, MyController::class)
```

#### Static

A static function can be called by providing the namespace followed by the function name concatonated with two colons `::` between the function name and the namespace.

```php
$dispatcher->dispatch('\My\Namespace::staticToCall', $staticToCallPara1, $staticToCallPara2, ...);
```

### Closure

The closure simply takes a closure as first parameter which will be called and executed during the dispatching process.

```php
$dispatcher->dispatch(function($para1, $para2) { ... }, $staticToCallPara1, $staticToCallPara2, ...);
```

## Routing

The dispatcher accepts the results from the `route()` method of the router which is an array of routes.

```php
$dispatcher->dispatch($router->route($request->uri->getRoute()));
```

Based on the function definition returned by the router it's possible to pass additional parameters to the function such e.g. request and response objects.

```php
$dispatcher->dispatch($router->route($request->uri->getRoute()), $request, $response);
```
