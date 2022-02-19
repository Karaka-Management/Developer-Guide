# DataMapper

## Models

Models can be constructed in what ever way you like, all of the mapping logic is defined in the data mapper itself. However, it is recommended to provide the following member variables if applicable (names can be different):

```php
private int $id = 0;
private \DateTime $createdAt;
private Account $createdBy;
```

The `$id` can be used as primary key. For this member variable no setter method should be present. For the `$createdAt` as well as the `$createdBy` member variables both getter and setter methods are possible. It's also possible to make these variables immutable since both should be known during the initialization point of a new model.

## DataMapper

The data mapper itself is where all the magic happens, from inserts, updates, to selects etc. In reality the data mappers are factories which create/populate the data mappers but since this is only internal behavior we will continue to call them mappers.

### Primary key

The primary key can be indicated with the constant `public const PRIMARYFIELD`. This constant should contain the string representation of the database field name. This constant is mandatory.

### Created at

While it is possible to log user, module and database activities through the logging module it is often necessary to know when a certain entry got created. For that purpose the `public const CREATED_AT` can be used to define the string representation of the database field name which contains the date of the insert.

### Autoincrement

In some cases it is necessary to define a primary key as not autoincrement. This messes with some parts of the write mapper. To let the mapper know that the model can have a user defined primary key you must set `public const AUTOINCREMENT = false`. By default it is `true`.

### Table

One model can only be stored in one table. With the `public const TABLE` constant it's possible to specify the table name. This constant is mandatory. It's important to note that by extending a model you also need to implement a data mapper that can access multiple tables. In that case it's also necessary to extend the data mapper of the extended module.

### Model

The `public const MODEL` constant can be optionally used in order to specify the model this mapper is supposed to populate. By default the mapper will try to find a model in the same directory as the mapper without the `Mapper` suffix in its name.

Default behavior example:

* Mapper name: `\test\path\TestMapper`
* Default model: `\test\path\Test`

If the model is defined somewhere else or has a different name, the `public const MODEL` constant name should be used to define the correct model. E.g.

```php
public const MODEL = OtherModel::class;
```

### Columns

In the `public const COLUMNS` array all columns, respective model constant and data types need to be specified.

```php
public const COLUMNS = [
    'db_field_name_1' => ['name' => 'db_field_name_1', 'type' => 'int',    'internal' => 'model_var_name_1'],
    'db_field_name_2' => ['name' => 'db_field_name_2', 'type' => 'string', 'internal' => 'model_var_name_2'],
];
```

The `name` contains the field name in the database, the `type` represents the data type and `internal` is the string representation of the model variable name.

#### Searchable columns

In order to make columns searchable you have to add `'autocomplete' => true` as column information to the respective column.

```php
public const COLUMNS = [
    'db_field_name_1' => ['name' => 'db_field_name_1', 'type' => 'int',    'internal' => 'model_var_name_1', 'autocomplete' => true],
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
* DateTimeImmutable
* Serializable (will call `serialize()`)
* Json (will call `jsonSerialize()`)

### Has many

With the `public const HAS_MANY` constant it's possible to specify other models that belong to this model.

```php
public const HAS_MANY = [
    'model_var_name_3' => [
        'mapper' => HasManyMapper::class,
        'table'  => 'relation_table_name',
        'dst'    => 'relation_destination_name',
        'src'    => 'relation_source_name',
    ],
];
```

The `mapper` contains the class name of the mapper responsible for the many models that belong to this model. The `table` contains the name of the table where the relations are defined (this can be the same table as the source model or a relation table). If a model is only in relation with one other model this relation can be defined in the same table as the model and this `table` field can be `null`. The `dst` field contains the name of field where the primary key of the destination is defined. The `src` field is only required for models which have a relation table. This field contains the name of the field where the primary key of the source model is defined.

#### Relation is defined in the model table

A many to one or one to one relation would look like the following:

```php
public const HAS_MANY = [
    'model_var_name_3' => [
        'mapper' => HasManyMapper::class,
        'table'  => null,
        'dst'    => 'relation_destination_name',
        'src'    => null,
    ],
];
```

#### Relation is defined in a relation table

A many to many relation which can only be defined in a relation table looks like the following:

```php
public const HAS_MANY = [
    'model_var_name_3' => [
        'mapper' => HasManyMapper::class,
        'table'  => 'relation_table_name',
        'dst'    => 'relation_destination_name',
        'src'    => 'relation_source_name',
    ],
];
```

#### Single field relations

By defining a `column` it's also possible to only populate the model with a single column/field value from another table or model.

```php
public const HAS_MANY = [
    'my_title' => [
        'mapper'      => L11nTagMapper::class,
        'table'       => 'tag_l11n',
        'external'    => 'tag_l11n_tag',
        'column'      => 'title',
        'conditional' => true,
        'self'        => null,
    ],
];
```

In the example above the model member variable `my_title` will be populated with the value `title` from the `L11nTagMapper`. Here the `L11nTagMapper` will to a reverse lookup of the column name for the variable `title` and return the content.

### Owns one

It's possible to also define a relation in the source module itself. This can be accomplished by using the `public const OWNS_ONE` constant. In this case the model itself has to specify a field where the primary key of the source model is defined.

```php
public const OWNS_ONE = [
    'model_var_name_4' => [
        'mapper' => OwnsOneMapper::class,
        'src'    => 'relation_dest_name',
    ],
];
```

The `mapper` field contains the class name of the mapper of the source model. The `src` field contains the database field name where the primary key is stored that is used for the relation.

### Belongs to

The reverse of a has one is a belongs to. This allows to also load models that a specific model belongs to. This can be accomplished by using the `public const BELONGS_TO` constant. In this case the model itself has to specify a field where the primary key of the source model is defined.

```php
public const BELONGS_TO = [
    'model_var_name_6' => [
        'mapper' => BelongsToMapper::class,
        'dest'   => 'relation_destination_name',
    ],
];
```

The `mapper` field contains the class name of the mapper of the destination model. The `dest` field contains the database field name where the primary key is stored that this model belongs to.

## Mapper interaction

The mapper interaction is similar to the query builder interaction.

### Reader

The read mapper allows you to read/select models from the database. The two most common use cases are `get()` and `getAll()`. The `get()` interaction returns a single model or an array of models if multiple models are found who fit the search criteria. The `getAll()` interaction always returns an array of models.

Example use case:

```php
$models = BaseModelMapper::get()->with(...)->with(...)->where(...)->sort(...)->limit(...)->execute();
```

##### Basic get

All interactions with the mappers are based on the property names of the models instead of the field names. The most simple read request would be to retrieve a model based on a property name (e.g. '$id'):

```php
$model = BaseModelMapper::get()->where('id', 12)->execute();
```

> Remark: By default no relations (e.g. has many, belongs to, owns one) are loaded. Make sure to use the `with()` function!

###### Writeonly

Write only properties are not loaded / remain empty. In order to force a load you may use the `with()` function.

##### Multiple filters

 Of course if no primary key is known you may also select by other properties or by multiple properties:

```php
$model = BaseModelMapper::get()->where('property1', 'firstValue')->where('property2', 'altValue', '=', 'or')->execute();
```

##### Limit result set

In order to limit the result set you may use the limit function:

```php
$models = BaseModelMapper::getAll()->where('property1', 'someValue')->limit(5)->execute();
```

##### Sort result set (and limit)

If the result order is important you can also define a order (e.g. get the newest 3 elements):

```php
$models = BaseModelMapper::getAll()->where('property1', 'someValue')->sort('id', OrderType::DESC)->limit(3)->execute();
```

##### Load relations (has many, belongs to, owns one)

By default no relations are loaded. This results in faster loading times and much more control over what data should be loaded. At the same time this requires more attention when using the data mappers. If relations aren't explicitly loaded they are initialized as null models in case of belongs to and owns one relationships. Has many relations result in empty arrays.

In order to load relations you must use the `with()` function:

```php
$model = BaseModelMapper::get()->with('ownsOnePropertyName')->where('id', 12)->execute();
```

It is normal for models to have sub-relations. Such models can be loaded by defining a path e.g.:

```php
$model = BaseModelMapper::get()
    ->with('ownsOnePropertyName')
    ->with('ownsOnePropertyName/childHasManyPropertyName')
    ->where('id', 12)
    ->execute();
```

Of course it's also possible to filter and limit these relations e.g.:

```php
$model = BaseModelMapper::get()
    ->with('ownsOnePropertyName')
    ->with('ownsOnePropertyName/childHasManyPropertyName')
    ->where('id', 12)
    ->where('ownsOnePropertyName/childHasManyPropertyName/someIntegerPropertyOfChild', 99, '<=')
    ->limit(4, 'ownsOnePropertyName/childHasManyPropertyName')
    ->execute();
```

#### Merge queries

In some cases you maybe want to provide a base query which the mapper should build upon. This base query is merged/extended in the mapper e.g.:

```php
$baseQuery = new Builder($con);
$baseQuery->innerJoin(...)->on(...)->where(...);

$model = BaseModelMapper::get()->query($baseQuery)->where('property1', 'test')->execute();
// since the $baseQuery is merged with the internal query generation you can use ->where(), ->limit(), etc. and they will be applied to the $baseQuery
```

This allows you to create better filtering than what is possible with the rudimentary where implementation of the mappers.

#### Overwrite query

Sometimes it may be necessary to ignore the internal query building process and create a model based on a custom query. Note that this function call DOESN'T merge the query which means you need to be very careful how you construct it so that it still correctly works with the mapper. One solution could be to first get the query built by the mapper and than modify that query e.g.:

```php
$query = BaseModelMapper::getQuery();
$query->innerJoin(...)->on(...)->where(...);

$model = BaseModelMapper::get()->execute($query);
// If you would specify a ->limit() here this would be ignored because it only uses the $query
```

#### Custom columns to load

In few cases you may only want to load specific columns only. This can be achieved with `columns(...)`. If the columns are defined only these columns are loaded e.g.:

```php
$model = BaseModelMapper::get()->columns(['table_id' => 'table_id_d1'])->where('id', 12)->execute();
```

> Remark: `columns()` actually requires column information, NOT the property name!

### Writer

The writer allows you to create a model in the database.

```php
$model = new BaseModel();

BaseModelMapper::create()->execute($model);
```

The writer goes through all relations and checks if they are already created in the database. If relations (e.g. has many, ...) are not created, the writer automatically creates them as well.

### Updater

The updater allows you to update a model in the database.

##### Basic update

The default update function works similar to the create function with the exception that relations (e.g. has many, belongs to, ...) are NOT automatically updated.

```php
$model = BaseModelMapper::get()->where('id', 12)->execute();
...
BaseModelMapper::update()->execute($model);
```

##### Update readonly/writeonly

Properties marked as either `readonly` or `writeonly` are not updated by default, use `with()` to force a different behavior.

###### Readonly

Readonly properties are supposed to not change. However, in order to force an overwrite you may use the `with(...)` function to do so.

###### Writeonly

Writeonly properties are not loaded during the `get()` call. Therefore they should also not get updated/overwritten with an empty value. However, in order to force an overwrite you may use the `with(...)` function to do so.

##### Update relations

In order to update relations you must specify that with `with()` e.g.:

```php
$model = BaseModelMapper::get()->with('ownsOnePropertyName')->where('id', 12)->execute();
...
BaseModelMapper::update()->with('ownsOnePropertyName')->execute($model);
```

This lets you specify very detailed what should get updated and what shouldn't get updated. Of course it also means you need to be careful when calling the updater.

### Remover

The updater allows you to delete a model from the database.

##### Basic delete

The default delete function deletes the model itself.

```php
$model = BaseModelMapper::get()->where('id', 12)->execute();
...
BaseModelMapper::delete()->execute($model);
```

The delete function doesn't delete owns one and belongs to relations. However, has many relations are handled a little bit more complicated.

###### Has many relations with relation table

If a has many relationship is defined in a relation table, then the relation is deleted from the relation table. However, the related module is not deleted as it might also be related to other models.

###### Has many relations with relations defined in the related model (not in a relation table)

If a relation is defined in the related model, than this model is also delete. The reason for this is that it cannot remain while the "parent" model is deleted (the foreign key would fail).