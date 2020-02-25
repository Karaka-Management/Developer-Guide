# Inspections

Code inspections are very important in order to maintain the same code quality throughout the application. The Build repository contains all esential configuration files for the respective inspection tools. Every provided module will be evaluated based on the predefined code and quality standards. Only modules that pass all code, quality and unit tests are accepted. This also applies to updates and bug fixes. Any change will have to be re-evaluated.

## Tools

Tools used for the code inspection are:

* PHPUnit
* PHPStan
* Jasmine
* PHPCS
* Custom scripts/tools

These tools are all installed by running the `setup.sh` script from the Build repository.

## PHPUnit

This application uses PHPUnit as unit testing framework. Unit tests for specific classes need to be named in the same manner as the testing class.

In order to run all tests and also creating the dummy data for UI tests, execute the following command:

```sh
php vendor/bin/phpunit -c tests/PHPUnit/phpunit_no_coverage.xml
```

In order to also create a code coverage report run:

```sh
php vendor/bin/phpunit -c tests/PHPUnit/phpunit_default.xml
```

### Modules

Every module needs to have a `Admin` directory containing a class called `AdminTest.php` which is used for testing the installation, activation, deactivation, uninstall and remove of the module. Tests that install, update, remove etc. a module need to have a group called `admin`. After running the `AdminTest.php` test the final state of the module should be installed and active, only this way it's possible to further test the controller and models. A code coverage of at least 80% is mandatory for every module for integration.

## PHPStan

With phpstan the code base is statically analyzed based on its configuration. This will help you to follow some of the "best" practices we enforce.

```sh
php vendor/bin/phpstan analyse --autoload-file=phpOMS/Autoloader.php -l 8 -c Build/Config/phpstan.neon --error-format=prettyJson ./ > Build/test/phpstan.json
```

## Jasmine

The javascript testing is done with jasmine. The javascript testing directory is structured the same way as the `Framework`. Unit tests for specific classes need to be named in the same manner as the testing class.

## PHP CS

Besides the code tests and static code analysis the code style is another very imporant inspection to ensure the code quality.

```sh
php vendor/bin/phpcs ./ --standard="Build/Config/phpcs.xml" -s --report-junit=Build/test/junit_phpcs.xml
```

## Git Hooks (Linux only)

The git hooks perform various checks and validations during the `commit` and warn the developer about invalid code or code style/guideline violations.

For developers it is recommended to copy the contents of the `default.sh` file in the `Build` repository under `Hooks` to your `pre-commit` file in the `.git/hooks` directory. If the `pre-commit` file doesn't exist just create it.

The same should be done with every module. Simply go to `.git/modules/**/hooks` and also add the content of the `default.sh` file to all `pre-commit` files.

By doing this every commit will be inspected and either pass without warnings, pass with warnings or stop with errors. This will allow you to fix code before committing it. Be aware only changed files will be inspected. Also make sure all `pre-commit` have `+x` permissions.