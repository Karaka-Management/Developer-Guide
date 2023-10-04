# Language Files

Language files are used for static/custom translations. Depending on the request and response localization the correct language files are loaded and used for the translations invoked by `getHtml()` in the views.

## Naming

The naming convention of the files is `language.lang.php` e.g. `en.lang.php`. In some cases it may be necessary to provide language files for other modules in such a case the name of the language file is `module_name.language.lang.php` e.g. `Navigation.en.lang.php`.

## Content

The content of the language files is very simple. All you need is the module name, a key/identifier for the string and the translation.

```php
<?php
/**
 * Karaka
 *
 * PHP Version 8.1
 *
 * @copyright Dennis Eichhorn
 * @license   OMS License 1.0
 * @version   1.0.0
 * @link      https://jingga.app
 */
declare(strict_types=1);

return ['Auditor' => [
    'Audit'   => 'Prüfung',
    'Auditor' => 'Prüfer',
    'Audits'  => 'Prüfungen',
    'By'      => 'Von',
]];
```

It is recommended to create the translations in a spreadsheet software and export it as csv, as it is much easier to create all the translations for the different languages. You can install the **demo application**, install **your module** and then go to the Exchange module. In here you can navigate to the **OMS** exchange (exporter) and export the language/translations. This will provide a csv file for all modules and all templates with all the different supported languages. Simply search for your module and create the missing translations.

Afterwards you can import the modified csv file in the OMS exchange which will create the language files based on this file.

> Please note that the csv must be **;** deliminated and **"** escaped.

## Import/Export

In order to create translations more easily you may use the OMS language exporter. This exporter generates a csv including all module langauge files. You can use this to easily check which localized string are not implemented in the language file of the respective language.

After you modified the csv file you can import it with the OMS language importer.

Since the import/export is so simple it is actually recommended to use `$this->getHtml('...')` in the `.tpl.php` files without manually generating a language file and only do it in the generated csv file. This option also allows you to use automatic translation tools.
