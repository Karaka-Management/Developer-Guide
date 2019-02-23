# Routing

Routing allows to bind a string representation to a function. This is required in order to execute request specific code segments.
Routes are defined in a uniform manner for all different application types such as http, socket or console.

## Routes

Routes are defined as RegEx. It is recommended to match the desired route as closely as possible and provide both `^` at the beginning and `$` at the end of the route.

Resolving a route can be done by providing a request to the router

```php
$router->route(new Request());
```

or a route

```php
$router->route('foo/bar', RouteVerb::GET);
```

The result is an array of either string references or closures.

## Closure

For routes it's possible to define a `\Closure` which will get returned upon using the specified route.

```php
$router->add('foo/bar', function() { 
	return 'Hellow World';
});
```

Routes can have different verbs which are derived from the HTTP verbs. Routes that get assigned a verb will only be matched if the route and the routing verb match.

```php
$this->router->add('foo/bar', function() { 
	return 'Hellow World';
}, RouteVerb::GET | RouteVerb::SET);
```

## Route Parameters

<coming soon>

## Reference

Instead of defining closures it's possible to define a string representation of the destination that should be called.

```php
$this->router->add('foo/bar', '\foo\controller:barFunction');
```

Static functions can be defined in the following fashion:

```php
$this->router->add('foo/bar', '\foo\controller::barFunction');
```

## Import

While routes can be added manually to the router it's also possible to import a list of routes through the file import function.

```php
$this->router->importFromFile($path);
```

The routing file must have the following structure:

```php
<?php return [
	'{ROUTE_STRING}' => [
		[
			'dest' => {CLOSURE/REFERENCE_STRING},
			'verb' => {VERB_1 | VERB_2},
		],
		[
			'dest' => {CLOSURE/REFERENCE_STRING},
			'verb' => {VERB_3},
		],
	],
	'{ANOTHER_ROUTE_STRING}' => [ ... ],
];
```

In this schematic the first route has different destinations depending on the verb.

## Permissions

It's also possible to define permissions for a specific application, organization and account in the routes which are then checked if they are set. If they are not set no permissions are checked.

```php
$router->route('foo/bar', RouteVerb::GET, 'APP_NAME', ORG_ID, ACCOUNT);
```

```php
<?php return [
	'{ROUTE_STRING}' => [
		[
			'dest' => {CLOSURE/REFERENCE_STRING},
			'verb' => {VERB_1 | VERB_2},
			'permission' => [
				'module' => {MODULE_NAME},
				'type' => {CREATE | READ | UPDATE | DELETE | PERMISSION},
				'state' => {MODULE_SPECIFIC_IDENTIFIER_FOR_THE_PERMISSION},
			],
		],
		[
			'dest' => {CLOSURE/REFERENCE_STRING},
			'verb' => {VERB_3},
		],
	],
	'{ANOTHER_ROUTE_STRING}' => [ ... ],
];
```

### Module Name

The module name has to be the name specified by the module. This can be found in the `info.json` or the `Controller` of the module.

### Type

The different permission types can be found in the `PermissionType` class/enum.

### State

This value is module specific and needs to be defined by every module individually.

The state allows a module to have different permissions. E.g. a news module has permissions for managing news articles (CRUDP), at the same time it also has permissions for managing tags/badges (CRUDP). Just because a user has permissions for managing news articles this user may not need to have the same permissions for managing tags/badges.
