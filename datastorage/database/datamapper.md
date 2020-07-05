# DataMapper

## Models

Models can be constructed in what ever way you like, all of the mapping logic is defined in the data mapper itself. However, it is recommended to provide the following member variables if applicable (names can be different):

```php
private $id = 0;
private $createdAt = null;
private $createdBy = null;
```

The `$id` can be used as primary key. For this member variable no setter method should be present. For the `$createdAt` as well as the `$createdBy` member variables both getter and setter methods are possible. It's also possible to make these variables immutable since both should be known during the initialization point of a new model.

## DataMapper

The data mapper itself is where all the magic happens, from inserts, updates, to selects etc.

### Primary key

The primary key can be indicated with the variable `$primaryField`. This variable should contain the string representation of the database field name. This variable is compulsory.

### Created at

While it is possible to log user, module and database activities thorugh the logging module it is often necessary to know when a certain entry got created. For that purpose the `$createdAt` can be used to define the string representation of the database field name which contains the date of the insert.

### Created by

In a similar fashion as the `$createdAt` variable often it is also necessary to have a field containing the id of the account creating the entry. The varibale `$createdBy` has to contain the string representation of that database field name.

### Table

One model can only be stored in one table. With the `$table` variable it's possible to specify the table name. This variable is compulsory. It's important to note that by extending a model you also need to implement a data mapper that can access multiple tables. In that case it's also necessary to extend the data mapper of the extended module.

### Model

The `$model` variable can be optionally used in order to specify the model this mapper is supposed to populate. By default the mapper will try to find a model in the same directory as the mapper without the `Mapper` suffix in its name.

Default behavior example:

* Mapper name: `\test\path\TestMapper`
* Default maodel: `\test\path\Test`

If the model is defined somewhere else or has a different name, the `$model` variable name should be used to define the correct model. E.g.

```php
protected static string $model = OtherModel::class;
```

### Columns

In the `$columns` array all columns, respective model variables and data types need to be specified.

```php
protected static array $columns = [
    'db_field_name_1' => ['name' => 'db_field_name_1', 'type' => 'int', 'internal' => 'model_var_name_1'],
    'db_field_name_2' => ['name' => 'db_field_name_2', 'type' => 'string', 'internal' => 'model_var_name_2'],
];
```

The `name` contains the field name in the database, the `type` represents the data type and `internal` is the string representation of the model variable name.

#### Searchable columns

In order to make columns searchable you have to add `'autocomplete' => true` as column information to the respective column.

```php
protected static array $columns = [
    'db_field_name_1' => ['name' => 'db_field_name_1', 'type' => 'int', 'internal' => 'model_var_name_1', 'autocomplete' => true],
    'db_field_name_2' => ['name' => 'db_field_name_2', 'type' => 'string', 'internal' => 'model_var_name_2'],
];
```

#### Types

Possible types are:

* int
* string
* bool
* float
* DateTime
* Serializable (will call `serialize()`)
* Json (will call `jsonSerialize()`)

### Has many

With the `$hasMany` variable it's possible to specify other models that belong to this model.

```php
protected static array $hasMany = [
    'model_var_name_3' => [
        'mapper'         => HasManyMapper::class,
        'table'          => 'relation_table_name',
        'dst'            => 'relation_destination_name',
        'src'            => 'relation_source_name',
    ],
];
```

The `mapper` contains the class name of the mapper responsible for the many models that belong to this model. The `table` contains the name of the table where the relations are defined (this can be the same table as the source model or a relation table). If a model is only in relation with one other model this relation can be defined in the same table as the model and this `table` field can be `null`. The `dst` field contains the name of field where the primary key of the destination is defined. The `src` field is only required for models which have a relation table. This field contains the name of the field where the primary key of the source model is defined.

#### Relation is defined in the model table

A many to one or one to one relation would look like the following:

```php
protected static array $hasMany = [
    'model_var_name_3' => [
        'mapper'         => HasManyMapper::class,
        'table'          => null,
        'dst'            => 'relation_destination_name',
        'src'            => null,
    ],
];
```

#### Relation is defined in a relation table

A many to many relation which can only be defined in a relation table looks like the following:

```php
protected static array $hasMany = [
    'model_var_name_3' => [
        'mapper'         => HasManyMapper::class,
        'table'          => 'relation_table_name',
        'dst'            => 'relation_destination_name',
        'src'            => 'relation_source_name',
    ],
];
```

#### Single field relations

By defining a `column` it's also possible to only populate the model with a single column/field value from another table or model.

```php
protected static array $hasMany = [
    'my_title' => [
        'mapper'   => L11nTagMapper::class,
        'table'    => 'tag_l11n',
        'external' => 'tag_l11n_tag',
        'column'   => 'title',
        'conditional'   => true,
        'self'     => null,
    ],
];
```

In the example above the model member variable `my_title` will be populated with the value `title` from the `L11nTagMapper`. Here the `L11nTagMapper` will to a reverse lookup of the column name for the variable `title` and return the content.

### Owns one

It's possible to also define a relation in the source module itself. This can be accomplished by using the `$ownsOne` variable. In this case the model itself has to specify a field where the primary key of the source model is defined.

```php
protected static array $ownsOne = [
    'model_var_name_4' => [
        'mapper' => OwnsOneMapper::class,
        'src'    => 'relation_dest_name',
    ],
];
```

The `mapper` field contains the class name of the mapper of the source model. The `src` field contains the database field name where the primary key is stored that is used for the relation.

### Belongs to

The reverse of a has one is a belongs to. This allows to also load models that a specific model belongs to. This can be accomplished by using the `$belongsTo` variable. In this case the model itself has to specify a field where the primary key of the source model is defined.

```php
protected static array $belongsTo = [
    'model_var_name_6' => [
        'mapper' => BelongsToMapper::class,
        'dest'   => 'relation_destination_name',
    ],
];
```

The `mapper` field contains the class name of the mapper of the destination model. The `dest` field contains the database field name where the primary key is stored that this model belongs to.

## Conditionals

Conditionals provide a general way to filter the desired result of models. You can think about conditionals as SQL `WHERE` clauses. A conditional uses the model member variable name instead of the database table name.

```php
WithConditionalMapper::withConditional('language', 'en')::getAll();
```

This query returns all models of the `WithConditionalMapper` which have a member variable with the value `en`. In the background the mapper does a reverse lookup, checks the column name which is associated with the `language` member variable and filters the database result accordingly.

By default conditionals are recursive and get applied to all models which are somehow referenced in the request (e.g. has many models which have also a `language` member variable). In some cases this is undesired and the user wants to specify the models where the conditionals should be applied to. This can be achieved in the following way:

```php
WithConditionalMapper::withConditional('language', 'en', [FirstModelToApplyTo::class, SecondModelToApplyTo::class, ...])::getAll();
```