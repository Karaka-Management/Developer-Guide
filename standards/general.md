# Code Standards

The term "class" refers to all classes, interfaces, and traits. These standards only show how the code should look like and doesn't give examples of bad code.

## Side Effects

A file SHOULD declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it SHOULD execute logic with side effects, but SHOULD NOT do both.

The phrase "side effects" means execution of logic not directly related to declaring classes, functions, constants, etc., merely from including the file.

"Side effects" include but are not limited to: generating output, explicit use of require or include, connecting to external services, modifying ini settings, emitting errors or exceptions, modifying global or static variables, reading from or writing to a file, and so on.

## Array

Arrays should always bet initialized by using `[]`.

```php
$arr = [1, 2, 3];
```

## Indention

The default indention MUST be 4 spaces.

## Spacing

### Enumerations

Always use a whitespace **after**:

* `,`
* `;` (unless at the end of a line)

### Operators

Always use a whitespace **before** and **after**:

* assignment (e.g. `=`, `=>`, `+=`, `/=`, `*=`, `-=`, `.=`)
* math operations (e.g. `+`, `-`, `*`, `/`, `%`, `&`, `|`, `**`, `>>`, `<<`)
* logic operators (e.g. `&&`, `||`)
* comparison (e.g. `==`, `===`, `>`, `>=`, `<`, `<=`)

### Other

Never use spaces between variables and atomic operations (e.g. `!`, `++`, `--`)

### Parentheses

Don't use whitespace inside ANY parentheses (e.g. functions, loops, conditions, catch, closures).

```js
for (let i = 1; i < 100; ++i) { ... }
```

```js
function(para1, para2) { ... }
```

### Brackets

Don't use whitespace inside ANY brackets.

```php
$arr = [1, 2, 3];
```

### Braces

Always use a whitespace between braces and keywords (e.g. else, while, catch, finally) and parentheses

```php
try {

} catch (...);
```

```php
if (...) {

} else (...);
```

Braces are on the same line as the previous or following keyword except in classes and functions.

```php
function()
{

}
```

```php
class Test
{
}
```

### If, while, for, foreach, switch

Always use a whitespace before the parentheses.

```php
while (true) {
    ...
}
```

## If, while, for, foreach, switch

Always use braces even for potential one liners. The only exception is the ternary operator.

```php
if (...) {

}
```

```php
$result = condition ? expr1 : expr2;
```

Multiline `if` conditions should begin with a logical opperator `or`/`and`.

```php
if (isTrue == true
    || isFalse === false
    && isNotTrue !== true
) {

}
```

Switch statements must have a `default` case.

## Naming

### Constants

Constants must be written with capital letters and snake case.

```js
CONSTANT_TEST = true;
```

### Function

Functions must be written in camelCase.

### Variables

Variables must be written in camelCase.

#### Boolean variables

Boolean variable names should start with a boolean expression (e.g. is, has)

##### Boolean return value

Functions which return a boolean value should start with expressions such as is*, has* or similar expressions to indicate that the function returns a boolean value.

### Database

#### Tables

Tables must be written in snake_case

#### Fields / Columns

Fields/columns must be written in snake_case and must be preceded by the table name.

```sql
table_name_field_name
```

This allows every field/column name to be unique.

## Quotation

All string representations should use single quotes `''` unless `""` provides significant benefits in a specific use case.

```js
'This is a string'
```

## Todos

Most issues should be documented in the code as todo and vice versa.

```php
/**
 * @todo Orange-Management/Repository#IssueNumber
 *  Below comes the issue/todo description.
 *  This way developers can see todos directly in the code without going to an external source.
 *  Todos must not have empty lines in their descriptions.
 *  If the external ressources have empty lines they must be removed in the todo comment.
 *      1. list item 1
 *      2. list item 2
 */
```