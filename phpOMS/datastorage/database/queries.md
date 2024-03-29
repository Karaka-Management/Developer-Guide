# Queries

## Database

The database query builder provides a uniform way to write default database queries for the following databases:

* MySQL
* PostgreSQL
* SQLite
* MicrosoftSQL
* OracleSQL

### Query Builder

The query builder is used for regular CRUD operations on the database.

#### Select, Insert, Update, Delete

Both `select` and `insert` expect the column names as parameter. The `where`, `from` and `into` clause can be necessary depending on the type of operation like a normal sql query.

```php
$query->select('columnA', 'columnB')->from('table')->where(...);
$query->insert('columnA', 'columnB')->values('a', 'b')->into('table');
```

The `update` expects the table name which should be updated and then the `set` function to define the columns and new values.

```php
$query->update('table')->set(['columnA' => 'a'])->set(['columnB' => 'b'])->where(...);
```

The `delete` function only expects the `from` and `where` clause to identify the to delete columns in a table.

```php
$query->delete()->from('table')->where(...);
```

##### Random

#### From

The `from` part of a query accepts `string`, `array`, `\Closure`, `From`, `Builder` as parameter.

```php
$query->select(...)->from('table');
$query->select(...)->from('tableA', 'tableB');
```

#### Into

The `into` part of a query accepts `string`, `array`, `\Closure`, `Into`, `Builder` as parameter.

#### Where

The basic `where` clause expects a column, operator, value and boolean concatenater which is used to concatenate multiple where clauses.

```php
$query->select(...)->from(...)->where('columnA', '=', 123)->where('columnB', '=', 'abc', 'or');
```

For easier use additional `where` clauses are defined such as:

* `orWhere()` - same as `where` with `or` as default boolean connector
* `andWhere()` - same as `where` with `and` as default boolean connector
* `whereIn()` - uses the sql `in(...)`
* `whereNull()` - used for null condition
* `whereNotNull()` - used for not null condition

#### Limit

The `limit` expects an integer.

```php
$query->select(...)->from(...)->where(...)->limit(3);
```

#### Offset

The `offset` expects an integer.

```php
$query->select(...)->from(...)->where(...)->offset(3);
```

#### Order

The ordering is performed by `orderBy`.

```php
$query->select(...)->from(...)->where(...)->orderBy('columnA', 'DESC');
```

The `newest` and `oldest` operation are a small wrapper which automatically order by `DESC` and `ASC` respectively.

#### Group By

Grouping of columns can be achieved through `groupBy`.

```php
$query->select(...)->from(...)->where(...)->groupBy('columnA', 'columnB');
```

#### Join

### Schema Builder

The schema builder is used for schema related operations such as `DROP`, `CREATE` etc.

#### Drop Database

A database can be dropped with `dropDatabase`.

```php
$query->dropDatabase('test');
```

#### Create Table

A table can be created with `createTable`.

```php
$query->createTable('user_roles')
    ->field('user_id', 'INT', null, false, true, true, 'users', 'ext1_id')
    ->field('role_id', 'VARCHAR(10)', '1', true, false, false, 'roles', 'ext2_id');
```

#### Show Tables

All tables of a database can be returned with `selectTables`.

```php
$query->selectTables();
```

#### Show Table Fields

All table fields of a table can be returned with `selectFields`.

```php
$query->selectFields('test');
```
