# Documentation

## Module

Every module must have two types of documentation. One for the end-user, which also includes information for the administrator and module configuration and another documentation for developers. Documentation must be available in at least English language.

### User

The user documentation should cover the following aspects:

* Installation, update, status change and delete process
* Dependencies and requirements
* Configuration options
* Permission management
* How to use the module and its functionality from an end-user perspective

### Developer

The developer documentation should cover the following aspects:

* Database UML diagram (incl. relations to other modules)
* How to interact with the controllers, views and models from other modules
* How to interact with the controllers, views and models from within the module
* Best practices (does and don'ts)
* Module specific guidelines & contribution information

## Database

### Diagrams

Every module that has a database table must implement a UML diagram illustrating the internal table relations as well as the relations with external tables from other modules.

## Php

The php documentation is based on PhpDocumentor, therefore only valid PhpDocumentor comments are valid for files, classes, functions/methods and (member) variables.

### File

A file documentation MUST be implemented in the following form:

```php
/**
 * File description
 *
 * PHP Version 7.0
 *
 * @package   Package name
 * @copyright Karaka
 * @license   OMS License 1.0
 * @version   1.0.0
 * @link      http://your.url.com
 */
```

### Class

A class documentation MUST be implemented in the following form:

```php
/**
 * Class description.
 *
 * @package Package name
 * @license OMS License 1.0
 * @link    http://your.url.com
 * @since   1.0.0
 */
```

#### Member

A member variable documentation MUST be implemented in the following form:

```php
/**
 * Member variable description.
 *
 * @var variable_type
 * @since 1.0.0
 */
```

#### Function/Method

A function/method documentation MUST be implemented in the following form:

```php
/**
 * Function/method description.
 *
 * Optional example or more detailed description.
 *
 * @param variable_type $param1Name Parameter description
 * @param variable_type $param2Name Parameter description
 *
 * @return return_type
 *
 * @since  1.0.0
 */
```

### Variable

Variable documentation is not mandatory and can be omitted. However it's recommended to use a variable documentation for objects and arrays of objects in templates for IDE code completion.

Example:

```php
/** @var TestObject[] $myArray */
```

## JavaScript

The javascript documentation is based on JsDoc, therefore only valid JsDoc comments are valid for all js files.

### File

```js
/**
 * File description.
 *
 * @package   Package name
 * @copyright Karaka
 * @license   OMS License 1.0
 * @version   1.0.0
 * @link      http://your.url.com
 */
```

### Class

A class documentation MUST be implemented in the following form:

```js
/**
 * Class description.
 *
 * @package Package name
 * @license OMS License 1.0
 * @link    http://your.url.com
 * @since   1.0.0
 */
```

#### Member

#### Function/Method

A function/method documentation MUST be implemented in the following form:

```js
/**
 * Function/method description.
 *
 * Optional example or more detailed description.
 *
 * @param {variable_type_1} param1Name     Parameter description
 * @param {variable_type}   [optionalPara] Parameter description
 *
 * @return {return_type}
 *
 * @since  1.0.0
 */
```

Please also note the correct alignment of `@param` type, name and description

### Variable

In some cases it may be required to type hint a variable in this case the following format MUST be used.

```php
/** @var variable_type varName {optional_description}
```

## Todos

Todos should be documented in the [todo list](https://github.com/orgs/Karaka-Management/projects/10).

In code todos can be created like this

```php
// @todo Single line todo
// @bug Single line bug
// @security Single line security
```

```php
/**
 * @todo Multi line todo
 *      This way developers can see todos directly in the code without going to an external source.
 *      Todos must not have empty lines in their descriptions.
 *      If the external resources have empty lines they must be removed in the todo comment.
 *          1. list item 1
 *          2. list item 2
 */
```

We support and recognize the following todo tags:

* @security For todos which have a strong security impact
* @bug For bugs
* @feature For features that should be implemented
* @performance For ideas/concerns regarding performance
* @todo General todos
* @question Ideas and concerns that need further investigation
