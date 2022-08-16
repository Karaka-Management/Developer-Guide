# Logging

The core provides two different logging outputs, one is server side file logging for exceptions, warnings etc. and the other one is for front end logging.

- [Server side file logging](#server-side-file-logging)
- [Client side logging](#client-side-logging)

## Server side file logging

The file logging should only be used for database and application problems. The file logging is part of the framework and is always available. The logging class implements the [LoggerInterface](https://github.com/Karaka-Management/phpOMS/blob/develop/Log/LoggerInterface.php).

### Initialization

```php
use phpOMS\Log\FileLogger;

$log = new FileLogger(string $lpath = '', bool $verbose = false);

// or
$log = FileLogger::getInstance(string $lpath = '', bool $verbose = false);

// or accessable through the application object
$app->logger;
```

* **(string) $lpath**: Path to the logging *directory* or logging *file*. If the path is a directory the logger will automatically create log files in the format $lpath/Y-m-d.
* **(bool) $verbose**: If true also outputs the logs to the current output buffer.

### Logging

```php
$log->emergency(string $message, array $context = []);
$log->alert(string $message, array $context = []);
$log->critical(string $message, array $context = []);
$log->error(string $message, array $context = []);
$log->warning(string $message, array $context = []);
$log->notice(string $message, array $context = []);
$log->info(string $message, array $context = []);
$log->debug(string $message, array $context = []);
$log->log(string $level, string $message, array $context = []);
```

* **(string) $message**: Message or message format used for logging.
* **(array) $context**: Context used to populate the message.

#### Example

```php
$log->alert(FileLogger::MSG_BACKTRACE, ['message' => 'Test log.']);
// Output: "2022-01-01 08:23:45"; "alert"; "127.0.0.1"; "Test log."; ...

$log->alert("Alert log.");
// Output: Alert log.

$log->log(FileLogger::MSG_BACKTRACE, ['message' => 'Test log.'], \phpOMS\Log\LogLevel::WARNING);
// Output: "2022-01-01 08:23:45"; "warning"; "127.0.0.1"; "Test log."; ...
```

#### Default messages

```php
FileLogger::MSG_BACKTRACE = '{datetime}; {level}; {ip}; {message}; {backtrace}';
FileLogger::MSG_FULL = '{datetime}; {level}; {ip}; {line}; {version}; {os}; {path}; {message}; {file}; {backtrace}';
FileLogger::MSG_SIMPLE = '{datetime}; {level}; {ip}; {message};';
```

The names in the *{}* brackets are used as identifiers in the context array.

The following identifiers have special meaning and are automatically populated in the logger:

* **backtrace**: Application backtrace
* **datetime**: Current datetime
* **level**: Populated based on the log function that got used (see [LogLevel](https://github.com/Karaka-Management/phpOMS/blob/develop/Log/LogLevel.php))
* **path**: `$_SERVER['REQUEST_URI']`
* **ip**: `$_SERVER['REMOTE_ADDR']`
* **version**: `\PHP_VERSION`
* **os**: `\PHP_OS`

#### References

* [Tests](https://github.com/Karaka-Management/phpOMS/blob/develop/tests/Log/FileLoggerTest.php)
* [Implementation](https://github.com/Karaka-Management/phpOMS/blob/develop/Log/FileLogger.php)

## Client side logging

On the client side a logger is also implemented providing the same functions as described above. The only difference is that this logger can remote log messages.

### Initialization

```js
import { Logger } from './jsOMS/Log/Logger.js';

let log = new Logger(bool verbose = true, bool ui = false, bool remote = true);

// or
let log = Logger.getInstance(bool verbose = true, bool ui = false, bool remote = true);

// or accessable through the window or application object
window.logger;
window.omsApp.logger;
```

* **verbose**: Defines if the log output will done in the console as well
* **ui**: Defines if the log output should be done as notification in the UI (uses the [Notification API](https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API))
* **remote**: Defines if the log output should be send to the backend (log endpoint is `/{lang}/api/log`)

### Logging

```js
log.emergency(string $message, object $context = {});
log.alert(string $message, object $context = {});
log.critical(string $message, object $context = {});
log.error(string $message, object $context = {});
log.warning(string $message, object $context = {});
log.notice(string $message, object $context = {});
log.info(string $message, object $context = {});
log.debug(string $message, object $context = {});
log.console(string $message, object $context = {});
log.log(string $level, string $message, object $context = {});
```

* **(string) message**: Message or message format used for logging.
* **(object) context**: Context used to populate the message.

#### Example

```js
log.alert(Logger.MSG_FULL, {'message': 'Test log.'});
// Output: "2022-01-01 08:23:45"; "alert"; "1.0.0"; "linux"; chrome"; "http://127.0.0.1"; Test log.";

log.alert("Alert log.");
// Output: Alert log.

log.log(Logger.MSG_FULL, {'message': 'Test log.'}, LogLevel.WARNING);
// Output: "2022-01-01 08:23:45"; "warning"; "1.0.0"; "linux"; chrome"; "http://127.0.0.1"; Test log.";
```

#### Default messages

```js
Logger.MSG_FULL = '{datetime}; {level}; {version}; {os}; {browser}; {path}; {message}';
```

The names in the *{}* brackets are used as identifiers in the context array.

The following identifiers have special meaning and are automatically populated in the logger:

* **backtrace**: Application backtrace
* **datetime**: Current datetime
* **level**: Populated based on the log function that got used (see [LogLevel](https://github.com/Karaka-Management/jsOMS/blob/develop/Log/LogLevel.js))
* **os**: Browser type (see [OSType](https://github.com/Karaka-Management/jsOMS/blob/develop/System/OSType.js))
* **browser**: Browser type (see [BrowserType](https://github.com/Karaka-Management/jsOMS/blob/develop/System/BrowserType.js))
* **path**: window.location.href
* **version**: 1.0.0

#### References

* [Tests](https://github.com/Karaka-Management/jsOMS/blob/develop/tests/Log/LoggerTest.js)
* [Implementation](https://github.com/Karaka-Management/jsOMS/blob/develop/Log/Logger.js)
