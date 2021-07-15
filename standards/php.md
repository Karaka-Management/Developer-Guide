# Php

The php code needs to be php 7 compliant. No php 7 deprecated or removed elements, functions or practices are allowed (e.g. short open tag). Please use the `phpcs.xml` and `phpmd.xml` configurations for PHP Code Sniffer and PHP Mess Detector provided with the project to identify most of the basic code standards.

##  Php Tags

PHP code MUST use the long `<?php ?>` tags or the short-echo `<?= ?>` tags; it MUST NOT use the other tag variations.

## Character Encoding

PHP code MUST use only UTF-8 without BOM

## Line Ending

Lines MUST end with `\n` (LF) and MUST NOT have a whitespace at the end.

## File Ending

Files must end with a new line element `\n`.

## Namespace and Class Names

Namespaces and classes MUST follow an "autoloading" PSR: [PSR-0, PSR-4].

This means each class is in a file by itself, and is in a namespace of at least one level: a top-level vendor name.

Class names MUST be declared in StudlyCaps.

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
	$para1,
	$para2,
	$para3,
	$para4
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

#### Elseif

Use `elseif` where possible instead of `else if`.

## Namespace

Namespaces must be surrounded with new line elements.

## Class Constants

Class constants MUST have a access modifier

```php
public CONST_NAME = ...;
```

## Preferred Functions

### file_exists

Instead of using `\file_exists()` the functions `\is_dir()` or `\is_file()` should be used.

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
