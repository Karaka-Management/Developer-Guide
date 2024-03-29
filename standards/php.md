# Php

The php code needs to be php 8.1 compliant. No php 8.1 deprecated or removed elements, functions or practices are allowed (e.g. short open tag). Please use the `phpcs.xml` configuration for PHP Code Sniffer to identify most of the basic code standards.

## Php Tags

PHP code MUST use the long `<?php ?>` tags or the short-echo `<?= ?>` tags; it MUST NOT use the other tag variations.

## Namespace and Class Names

Namespaces and classes MUST follow an "autoloading" PSR: [PSR-0, PSR-4].

This means each class is in a file by itself, and is in a namespace of at least one level: a top-level vendor name.

Class names MUST be declared in StudlyCaps.

Nativ PHP functions must use global namespacing e.g. `\count(...)`.

### Return type hint

The return type hint must have a whitespace after the closing braces and after the colon. The return type must be on the same line as the closing brace.

```php
function() : int
{

}
```

or for multiline function parameters

```php
function(
    string $para1,
    int $para2,
    bool $para3,
    array $para4
) : int
{

}
```

### Default Functions

Function calls to php internal function calls must use the root namespace `\`:

```php
\substr(...);
\is_bool(...);
\file_get_contents(...);
....
```

## Type hints

Type hints are mandatory wherever reasonably possible (member variables, function parameters, return types, ...).

## Attributes

Function attributes must not be used!

## Php in html

Php code embedded into template files SHOULD use the alternative syntax for control structures in order to improve the readability:

```php
<?php if ($a === 5) : ?>
    <p>This is html</p>
<?php endif; ?>
```

## Echo

when echoing multiple components, don't concat them but use `,`.

```php
echo 'Hello' , 'World';
```

## If

### Elseif

Use `elseif` where possible instead of `else if`.

## Namespace

Namespaces must be surrounded with new line elements.

## Class Constants

Class constants MUST have a access modifier

```php
public CONST_NAME = ...;
```

## Array

Arrays should always bet initialized by using `[]`.

```php
$arr = [1, 2, 3];
```

## Preferred Functions

### file_exists

Instead of using `\file_exists()` the functions `\is_dir()` or `\is_file()` should be used. `\file_exists()` should be only used if the programmer doesn't know if the provided path is a file or directory.

### String starts with / ends with

Instead of using `\stripos() === 0` to check if a string starts with a certain string use `\str_starts_with()`. Similarly use `\str_ends_with()`

### str_replace

Whenever possible instead of using `\str_replace()` use the function `\strtr()`. 

> Careful, \strtr() **DOES NOT** support deleting a character = replace it with an empty string!

### json_encode

For simple static arrays `\json_encode()` should be avoided and a manual json string generation should be used.

## Enum

Don't use the internal enum implementations of PHP (neither `SplEnum` nor `enum`)

### Pseudo enums

Whenever a scalar coming from the internal enum data type (`\phpOMS\Stdlib\Base\Enum`) is used the variable should be private and a getter and setter should exist for additional type checks.

```php
private int $myStatus = TestEnum::ACTIVE; // extends \phpOMS\Stdlib\Base\Enum
```

## Deprecated functions and variables

The following functions and (super-) global variables MUST NOT be used.

* `extract()`
* `parse_str()`
* `int_set()`
* `putenv()`
* `eval()`
* `assert()`
* `system()`
* `shell_exec()`
* `create_function()`
* `call_user_func_array()`
* `call_user_func()`
* `url_exec()`
* `passthru()`
* `Java()`
* `COM()`
* `event_new()`
* `dotnet_load()`
* `runkit_function_rename()`
* `pcntl_signal()`
* `pcntl_alarm()`
* `register_tick_function()`
* `dl()`
* `pfsockopen()`
* `fsockopen()`
* `posix_mkfifo()`
* `posix_getlogin()`
* `posix_ttyname()`
* `posix_kill()`
* `posix_mkfifo()`
* `posix_setpgid()`
* `posix_setsid()`
* `posix_setuid()`

The following functions and (super-) global variables MAY only be used in the phpOMS Framework in special cases.

* `$_GET`
* `$_POST`
* `$_PUT`
* `$_DELETE`
* `$_SERVER`
* `header()`
* `mail()`
* `phpinfo()`
* `getenv()`
* `get_current_user()`
* `proc_get_status()`
* `get_cfg_var()`
* `disk_free_space()`
* `disk_total_space()`
* `diskfreespace()`
* `getcwd()`
* `getlastmo()`
* `getmygid()`
* `getmyinode()`
* `getmypid()`
* `getmyuid()`
* `proc_nice()`
* `proc_terminate()`
* `proc_close()`
* `pfsockopen()`
* `fsockopen()`
* `apache_child_terminate()`
* `posix_kill()`
* `posix_mkfifo()`
* `posix_setpgid()`
* `posix_setsid()`
* `posix_setuid()`
* `ftp_get()`
* `ftp_nb_get()`
* `register_shutdown_function()`
* `chown()`
* `chdir()`
* `chmod()`
* `chgrp()`
* `symlink()`
* `flock()`
* `socket_create()`
* `socket_connect()`
