# Routing

Routing allows to bind a string representation to a function. This is required in order to execute request specific code segments. One very common scenario for routing is to take the URL of a http(s) request and assign it to a function located in a controller.

## Routes

Routes are defined as RegEx. It is recommended to match the desired route as closely as possible and provide both `^` at the beginning and `$` at the end of the route.

Resolving a route can be done by providing the route path e.g.

```php
$router->route('foo/bar', null, RouteVerb::GET); // returns an array of endpoints
```

The result is an array of either string references or closures.

## Adding Routes to the Router

Routes can be added individually in the code ore defined in an array which is stored in a file.

### Closure

For routes it's possible to define a `\Closure` which will get returned upon using the specified route.

```php
$router->add('foo/bar', function() {
	return 'Hello World';
});
```

Routes can have different verbs which are derived from the HTTP verbs. Routes that get assigned a verb will only be matched if the route and the routing verb match.

```php
$router->add('foo/bar', function() {
	return 'Hello World';
}, RouteVerb::GET | RouteVerb::SET);
```

### Reference

Instead of defining closures it's possible to define a string representation of the destination that should be called.

```php
$router->add('foo/bar', '\foo\controller:barFunction');
```

Static functions can be defined in the following way:

```php
$router->add('foo/bar', '\foo\controller::barFunction');
```

### Import from File

While routes can be added manually to the router it's also possible to import a list of routes through the file import function.

```php
$router->importFromFile($path);
```

The routing file must have the following structure:

```php
<?php return [
	'{ROUTE_STRING}' => [
		[
			'dest' => CLOSURE/REFERENCE_STRING,
			'verb' => VERB_1 | VERB_2,
		],
		[
			'dest' => CLOSURE/REFERENCE_STRING,
			'verb' => VERB_3,
		],
	],
	'{ANOTHER_ROUTE_STRING}' => [ ... ],
];
```

In this schematic the first route has different destinations depending on the verb.

## Verbs

With request verbs it's possible to use the same request path and assign different endpoints to them. This is helpful when you want to have the same path for retrieving data and changing data. This is a normal situation in web development e.g.

Let's assume we have the URL `https://yoururl.com/user/1/name` if we make a `GET` request we could return the name for the user `1` and if we make a `POST` request we could update the name for the user `1` and use the same url.

Allowed request verbs are:

* ::GET (e.g. for http get requests)
* ::SET (e.g. for http post requests which update existing data)
* ::PUT (e.g. for http put requests which create data)
* ::DELETE (e.g. for http delete requests which delete data)
* ::ANY (for every request if the verb doesn't matter)

In the routing definition it's possible to allow multiple verbs for the same endpoint by using `|` in the definition:

```php
'verb' => RouteVerb::GET | RouteVerb::POST
```

## Permissions

It's also possible to define permissions for a specific application, organization and account in the routes which are then checked if they are set. If they are not set no permissions are checked.

```php
$router->route('foo/bar', null, RouteVerb::GET, 'APP_NAME', ORG_ID, ACCOUNT);
```

```php
<?php return [
	'{ROUTE_STRING}' => [
		[
			'dest' => CLOSURE/REFERENCE_STRING,
			'verb' => VERB_1 | VERB_2,
			'permission' => [
				'module' => MODULE_NAME,
				'type' => CREATE | READ | UPDATE | DELETE | PERMISSION,
				'state' => MODULE_SPECIFIC_IDENTIFIER_FOR_THE_PERMISSION,
			],
		],
		[
			'dest' => CLOSURE/REFERENCE_STRING,
			'verb' => VERB_3,
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

## CSRF Protection

Often you would like to enable `CSRF` protection for certain urls or routing paths. The router can check if a CSRF protection is needed for a certain path and if it is needed it will check if the correct CSRF token is provided. If no CSRF protection is required it will ignore the CSRF token.

```php
$router->add('foo/bar', function() {
	return 'Hello World';
	}, RouteVerb::GET | RouteVerb::SET,
	true, // CSRF token is required
);
```

```php
$router->route(
	'foo/bar',
	$request->getData('CSRF'),
	RouteVerb::GET,
	'APP_NAME',
	ORG_ID, ACCOUNT
);
```

## Notes

* 2-character routes on the first level are discouraged because they may conflict with ISO 639-1 codes (e.g. /hr/staff/list)