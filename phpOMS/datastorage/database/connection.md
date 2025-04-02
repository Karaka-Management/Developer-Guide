# Database Connection

A database connection can be created manually with the respective database connection model, with the database connection factory or with the database pool which also manages multiple database connections.

## Database Connection

A connection takes an array which contains the database connection information and create the connection:

```php
$con = new MysqlConnection([
    'db'       => 'mysql', /* db type */
    'host'     => '127.0.0.1', /* db host address */
    'port'     => '3306', /* db host port */
    'login'    => 'root', /* db login name */
    'password' => 'root', /* db login password */
    'database' => 'oms', /* db name */
]);
```

With `getStatus()` you can check the status of the connection.

## Database Factory

The connection factory automatically creates the correct connection based on the database definition:

```php
$con = ConnectionFactory::create([
    'db'       => 'mysql', /* this is used by the connection factory to pick the correct connection */
    'host'     => '127.0.0.1',
    'port'     => '3306',
    'login'    => 'root',
    'password' => 'root',
    'database' => 'oms',
]);
```

### Database types

Available database types/constants can be found in the the `DatabaseType.php` file:

```php
public const MYSQL = 'mysql'; /* MySQL */
public const SQLITE = 'sqlite'; /* SQLITE */
public const PGSQL = 'pgsql'; /* PostgreSQL */
public const SQLSRV = 'mssql'; /* Microsoft SQL Server */
```

Example usage:
```php
DatabaseType::MYSQL;
```

## Database Pool

Existing database connections can be added to the database pool:

```php
$dbPool = new DatabasePool();
$dbPool->add('read', $con1);
$dbPool->add('write', $con2);

$dbPool->get('read'); // returns $con1
```

This allows to create different database connections with different permissions or handling database connections with different databases.

A new database connection can also be directly created by the database pool:

```php
$dbPool = new DatabasePool();
$dbPool->create('read',
    [
        'db'       => 'mysql',
        'host'     => '127.0.0.1',
        'port'     => '3306',
        'login'    => 'root',
        'password' => 'root',
        'database' => 'oms',
    ]
);
```