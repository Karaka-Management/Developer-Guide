# Modules

The following directory structure should roughly visualize how modules are structured. The purpose of the different sub-directories and their files will be covered in the following sections.

```
* {UniqueModuleName}
    * Admin
        * Install
            * db.json
            * Navigation.install.json
            * Navigation.php
            * ... more providing scripts similar to Navigation.*
        * Hooks
            * Web
                * Api.php
        * Routes
            * Cli.php
            * Web
                * Backend.php
                * Api.php
        * Update
            * yourUpdateFiles.???
        * Activate.php
        * Deactivate.php
        * Installer.php
        * Uninstall.php
        * Update.php
    * Controller
        * BackendController.php
        * ApiController.php
        * Controller.php
        * Controller.js
    * Docs
        * Dev
            * en
                * SUMMARY.md
                * ...
        * Help
            * en
                * SUMMARY.md
                * ...
    * Img
        * modulePreviewImage.jpg
    * Models
        * YourPHPModel.php
        * NullYourPHPModel.php
        * YourPHPModelMapper.php
        * ... more models/mappers/enums
        * YourJavaScriptModels.js
    * Theme
        * Backend
            * Css
                * yourCss_1.0.0.css
                * yourScss_1.0.0.scss
            * Img
                * yourTemplateImages.jpg
            * Lang
                * en.lang.php
                * Navigation.en.lang.php
            * your_template_files.tpl.php
    * Views
        * YourPhpViews.php
        * YourJavaScriptViews.js
    * info.json
    * README.md
```

All modules are located inside the `/Modules` directory and their directory name has to be the module name itself without whitespace.

## Admin

The admin directory contains the install directory as well as the install, delete, update, activate and deactivate script associated with this module. The install directory contains installation files required for other modules. The above example contains the two required files for providing navigation information to the navigation module so that the navigation module can display this module in the navigation bar. The navigation installation file as well as all other module installation files must have the same name as the navigation module and will be automatically called on installation if defined in the info.json file.

The content of the navigation install file highly depends on the module and should be documented in the according module. The additional json file is also required by the navigation module for the installation process. How many additional files and how they have to be structured/named should all be documented in the module documentation. If your module doesn't provide any navigation links or in general doesn't use any other modules, your install directory will be empty.

Some modules can be used without requiring any additional installations it all depends on how the other modules got implemented. Thats also why many modules don't offer any integration at all and
are almost stand-alone without the possibility to get extended.

### Database schema

The database schema of a module is defined in the `db.json` file and is automatically installed during the installation process.

In the following you can find a sample `db.json` file.

```json
{
    "task": {
        "name": "task",
        "fields": {
            "task_id": {
                "name": "task_id",
                "type": "INT",
                "null": false,
                "primary": true,
                "autoincrement": true
            },
            "task_title": {
                "name": "task_title",
                "type": "VARCHAR(255)",
                "null": false
            },
            "task_desc": {
                "name": "task_desc",
                "type": "TEXT",
                "null": false
            },
            "task_type": {
                "name": "task_type",
                "type": "TINYINT",
                "null": false
            },
            "task_created_at": {
                "name": "task_created_at",
                "type": "DATETIME",
                "default": null,
                "null": true
            },
            "task_created_by": {
                "name": "task_created_by",
                "type": "INT",
                "null": false,
                "foreignTable": "account",
                "foreignKey": "account_id"
            }
        }
    },
    "task_media": {
        "name": "task_media",
        "fields": {
            "task_media_id": {
                "name": "task_media_id",
                "type": "INT",
                "null": false,
                "primary": true,
                "autoincrement": true
            },
            "task_media_src": {
                "name": "task_media_src",
                "type": "INT",
                "null": false,
                "foreignTable": "task",
                "foreignKey": "task_id"
            },
            "task_media_dst": {
                "name": "task_media_dst",
                "type": "INT",
                "null": false,
                "foreignTable": "media",
                "foreignKey": "media_id"
            }
        }
    }
}
```

### Installer.php

In contrast to the install file for other modules this file has to follow more strict standards. The following example shows you the bare minimum requirements of a installation file:

```php
<?php
namespace Modules\Navigation\Admin;

use phpOMS\DataStorage\Database\DatabaseType;
use phpOMS\DataStorage\Database\DatabasePool;
use phpOMS\Module\ModuleInfo;
use phpOMS\Module\InstallerAbstract;

final class Installer extends InstallerAbstract
{
    public static function install(string $path, Pool $dbPool, ModuleInfo $info)
    {
        parent::install($path, $dbPool, $info);

        /* Your module specific setup goes here */
    }
}
```

If your application doesn't need to implement any database tables for itself the switch statement can be omitted. From the directory structure at the beginning we can however see that some modules accept information form other modules. The following example shows how the navigation module is accepting information during the installation of other modules:

```php
public static function installExternal(Pool $dbPool, array $data) : array
{
    /* What do you want to do with the data provided by $data (coming from other modules)? */
}
```

Install navigation elements: Modules have to create a Navigation.php file inside the install directory with the following method:

```php
public static function install(string $path, Pool $dbPool)
{
    $navData = \json_decode(file_get_contents(__DIR__ . '/Navigation.install.json'), true);

    $class = '\\Modules\\Navigation\\Admin\\Installer';
    $class::installExternal($dbPool, $navData);
}
```

How the receiving module (e.g. Navigation) is accepting information depends on the module itself. The module documentation will also state how the content of the `install(...)` method has to look like. At the same time if you write a module and are accepting information from other modules during their installation you have to document very well how they have to provide these information. Very often however it will not be necessary to let other modules pass these information during installation and only do this during runtime.

The navigation module is a good example of passing navigation links during installation. The navigation module could request the link information during runtime this would mean that all modules would have to be initialized for every request since the navigation module doesn't know if these modules are providing links or not. By providing these information during the installation, the navigation module can store these information in a database table and query these information for every page request without initializing all modules or performing some file readings.

### Update.php

### Uninstall.php

### Activate.php

### Deactivate.php

### Routes

In the routes directory all application routes are stored. These routing files get used during the installation process of every module and are stored in an application specific routing file. The first directory level contains the directories of the application type `Web`, `Socket` and `Console`. Inside these directories the routing files for the applications are stored by the name of the application e.g. `Backend.php`. The routing files themselves look like this:

```php
<?php

use phpOMS\Router\RouteVerb;

return [
    '^.*/backend/admin/settings/general.*$' => [
        [
            'dest' => '\Modules\Admin\Controller:viewSettingsGeneral',
            'verb' => RouteVerb::GET,
        ],
    ],

    '^.*/api/admin/module/status.*$' => [
        [
            'dest' => '\Modules\Admin\Controller:apiModuleStatusGet',
            'verb' => RouteVerb::GET,
        ],
        [
            'dest' => '\Modules\Admin\Controller:apiModuleStatusUpdate',
            'verb' => RouteVerb::SET,
        ],
    ],
];

```

The routes are defined via regex. One uri can have multiple destinations depending on the request type/verb. It's also possible to have multiple destinations for the same verb in case other views need to be loaded for the same uri (e.g. view injection in order to extend modules).

## Img

All module specific images (not theme specific images). E.g. Module preview images showing when searching for modules.

## Models

All models and data mapper classes should be stored in here (PHP & JS). How to create a data mapper for a model is described in the data mapper chapter. All JavaScript files need to be provided un-optimized (not minified or concatenated).

## Theme

The Theme directory contains the current theme for every page this module supports. If a module only supports the backend application there will only be a Backend directory containing the theme for the backend.

### Css

Every page has its own CSS directory. This application only allows the use of SASS/SCSS as preprocessor. All sass/scss files need to be provided as well as the processed CSS files. Make sure to update the version number in the `Controller.php` file. CSS files need to be minimized and if it makes sense concatenated.

### Img

This directory contains all images for this page.

### Lang

The Lang directory contains all language files for this application. Usually there is one language file for the page which will be loaded automatically wherever the module gets loaded (this language file has to exist).

A language file should have the following naming convention:

    {ISO 639-1}.lang.php
    {UniqueModuleName}.{ISO 639-1}.lang.php

The content of the language file is straight forward:

```php
<?php return [
    '{UniqueModuleName}' => [
        'StringID' => 'Your localized string',
        ...
    ]
];
```

All other language files are optional and usually are only required by other modules. The navigation module for example requires an extra language file for the navigation elements. This however should be specified in the modules you want to make use of.

## Views

## Controller.php

## Controller.js

## info.json

The `info.json` file contains general module information used during installation as well for identification and display in the module database.

```json
{
    "name": {
        "id": {UniqueModuleId_INT},
        "internal": "{UniqueModuleId_STR}",
        "external": "{Readable module name}"
    },
    "version": "{Version}",
    "requirements": {
        "phpOMS": "{Version}",
        "phpOMS-db": "{Version}"
    },
    "creator": {
        "name": "{Creator name/organization}",
        "website": "{Creator website}"
    },
    "description": "{Module description}",
    "directory": "{UniqueModuleId_STR}",
    "dependencies": {},
    "providing": {
        "Navigation": "{Version}"
    },
    "load": [
        {
            "pid": [
                "{Request_path}"
            ],
            "type": 4,
            "for": "Content",
            "file": "{UniqueModuleId_STR}",
            "from": "{UniqueModuleId_STR}"
        },
        {
            "pid": [
                "{Request_path}"
            ],
            "type": 5,
            "from": "{UniqueModuleId_STR}",
            "for": "{UniqueModuleId_For_STR}",
            "file": "{UniqueModuleId_For_STR}"
        },
        {
            "pid": [
                "{Request_path}"
            ],
            "type": 5,
            "for": "Content",
            "file": "{UniqueModuleId_STR}",
            "from": "{UniqueModuleId_STR}"
        }
    ]
}

```

### Name

The name category contains the names used for internal as well as external identification. Both `id` and `internal` are globally unique between all modules and is assigned generated by Karaka. The `id` can also be used by other modules which need integer identification of modules. The `id` has the form `xxxxx00000` which means, that modules don't only occupy a single id but a range of ids. This can be useful for other modules where additional module specific information need to be assigned (e.g. `Navigation` module). The `external` name is the name the creator of the module can give the module. This is the name that will be used during searches for modules and shown to customers.

### Version

The version is automatically incremented by Karaka. The version might be used by other modules in order to require a specific version of a module. Versions must follow the following format:

```js
major.minor.build = 2.512.19857
```

### Requirements

The `requirements` are used in order to specify core specific requirements such as the database requirements, framework requirements, third party library requirements as well as potential local software requirements.

### Creator

In the `creator` category it's possible to specify the `name` of the creator and the `website` of the creator. Multiple creators are not possible.

### Description

In the `description` a markdown description can be specified which will be shown in the module overview. This can be used in order to describe the users what the purpose of this module is and how it works.

### Directory

The `directory` is the directory name of the module which is usually the unique string representation of the module. This is the directory in the modules folder where the module files will be located.

### Dependencies

Often modules depend on other modules, these dependencies can be defined in the `dependencies`. The keys contain the unique string representation of the required modules and the value contains the version that is required.

### Providing

In the `providing` category modules can specify other modules they intend to extend if they are installed. The keys contain the unique string representation of the module which is getting extended and the value contains the version of the module that is supposed to get extended.

### Load

The `load` category contains all the files (controllers, language files) that need to be loaded during the application setup for a specific request uri.

#### PID

The `pid` (page identifier) is the request path. The pid needs to be defined in the following matter (e.g. http://127.0.0.1/en/app/path/sub&some=thing):

```json
'/app/path'
```

The result of the example would result in loading all specified resources for requests containing '/app/path'. This also includes requests to any subdirectories.

#### Type

The `type` contains the type of the file that needs to be loaded.

* 5 = Navigation language file
* 4 = Module controller

#### For

The `for` contains the unique module string representation the file is loaded for. This could be the own module or a third party module.

#### From

The `from`contains the unique module string representation the file is loaded from. This is the `internal` module name from the top of the `info.json` file.

#### File

The `file` contains the name of the file to be loaded. No path or extension is required nor allowed.
