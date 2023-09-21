# Validation

The built in validation allows to quickly validate some basic data formats.

## Guarding

The `Guard` provides basic protection function for comman mistakes and angles of attacks:

### Path guarding

Usually no user input should be used for path construction but in some cases this might not be possible. With the path guarding you can check if the generated path would lead outside the applications root path.

```php
public static function isSafePath(string $path, string $base = '') : bool;
```

By leaving the `$base` path empty the function automatically restricts the `$path` to the parent directory of the framework.

## Validator

The `Validator` provides basic validations with:

### isValid

With the `isValid` function you can provide a variable which should be validated and one or multiple constraints which should be checked. The return is a boolean.

```php
public static function isValid(mixed $var, array $constraints = null) : bool;
```

Example:

```php
function functionName(mixed $var, $mySetting1, $mySetting2) : bool
{
    // Do some validation here
    return true;
}

Validator::isValid($testVariable, ['functionName' => $settings]);
```

The second parameter (settings parameter) contains an associative array where the key is the function name which should get invoked and the value are the settings/options/parameters passed to the function name. The function must be implemented by the user.

If multiple constraints are provided the validation only returns true if all validations are successful.

In order to negate the validation simply add a `Not` to the function name.

> The negated function name with *Not* must not be separately implemented as it will be negated internally.

### hasLength

With `hasLength` you can check if a string has defined length.

```php
public static function hasLength(string $var, int $min = 0, int $max = \PHP_INT_MAX) : bool;
```

### hasLimit

With `hasLimit` you can check if a numeric value is in a defined range.

```php
public static function hasLimit(int | float $var, int | float $min = 0, int | float $max = \PHP_INT_MAX) : bool;
```

## Base

### DateTime

The `DateTime` validator checks if a specified value is a valid datetime.

```php
DateTime::isValid('2099-01-01');
```

### Json

The `Json` validator checks if a specified value is a valid json string.

```php
Json::isValid('[1, 2, 3]');
```

#### Templates

You can specify a json template which uses regex for validation. The template then is used to validate against a json array.

The validator checks for completeness, match against the template and possible additional specifications which are not part of the template.

Example for our `info.json` valdiation

```json
{
    "name": {
        "id": "^[1-9]\\d*",
        "internal": "[a-zA-Z0-9]+",
        "external": "[a-zA-Z0-9]+"
    },
    "category": "[a-zA-Z0-9]+",
    "version": "([0-9]+\\.){2}[0-9]+",
    "requirements": {
        ".*": ".*"
    },
    "creator": {
        "name": ".+",
        "website": ".*"
    },
    "description": ".+",
    "directory": "[a-zA-Z0-9]+",
    "dependencies": {
        ".*": ".*"
    },
    "providing": {
        ".*": ".*"
    },
    "load": [
        {
            "pid": [
                ".*"
            ],
            "type": "^[1-9]\\d*",
            "for": "^([1-9]\\d*)|([a-zA-Z0-9]+)",
            "from": "[a-zA-Z0-9]+",
            "file": "[a-zA-Z0-9]*"
        }
    ]
}

```

The example above shows how to specify optional elements (e.g. "dependencies") which can have as many children in any form possible or optional values with a specific key (e.g. website) which can be omitted.

## Finance

### BIC

A simple BIC validator which only validates the structure not if it actually exists.

```php
BIC::isValid('DSBACNBXSHA');
```

### Iban

A simple IBAN validator which only validates the structure not if it actually exists.

```php
Iban::isValid('DE22 6008 0000 0960 0280 00');
```

### CreditCard

A simple credit card validator which only validates the structure not if it actually exists.

```php
CreditCard::isValid('4242424242424242');
```

## Network

### Email

A simple email validator which only validates the structure not if it actually exists.

```php
Email::isValid('test.string@email.com');
```

### Hostname

A simple hostname validator which only validates the structure not if it actually exists.

```php
Hostname::isValid('[2001:0db8:85a3:0000:0000:8a2e:0370:7334]'); // true
Hostname::isValid('test.com'); // true

Hostname::isValid('2001:0db8:85a3:0000:0000:8a2e:0370:7334'); // false
Hostname::isValid('http://test.com'); // false
```

### Ip

A simple Ip validator which only validates the structure not if it actually exists.

```php
IP::isValid('192.168.178.1');
IP::isValid('2001:0db8:85a3:0000:0000:8a2e:0370:7334');

IP::testValidIp4('192.168.178.1');
IP::testValidIp6('2001:0db8:85a3:0000:0000:8a2e:0370:7334');
```

> Please note that the Ip4 and Ip6 validators only validate their corresponding Ips which means that a Ip4 will not correctly validate as Ip6 and vice versa.

## Enum

Enums can be validated by name/key and by value.

```php
MyEnum::isValidName('ENUM_NAME');
MyEnum::isValidValue(123);
```
