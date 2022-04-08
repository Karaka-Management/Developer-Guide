# Code Inspections & Tests

Code inspections are very important in order to maintain the same code quality throughout the application. The `Build` repository and package managers such as `composer` and `npm` contain all essential configuration files for the respective inspection tools. The framework and every module will be evaluated based on the defined code and quality standards. Only code that passes all code, quality and test standards are accepted. Updates and bug fixes also must follow the standards.

## How and what to test?

In this project multiple levels of tests must be implemented such as unit tests, integration tests and system tests.

The following testing requirements must be met:

* 90% code coverage in the tests
* all tests must pass without warnings, errors and exceptions
* no warnings and errors during static code inspections
* no usage of deprecated function calls
* no code style violations
* every test should have a short description for the test report
* every file containing code (except enums, traits, interfaces and template files) must have their own test file with tests

When testing it makes sense to test for the happy path/branch of how a method should work and to `try` and break things by trying to find inputs and paths which may lead to unexpected behavior and errors.

### Unit tests

The smallest/lowest level of testing are the unit tests. Unit tests should be implemented for models and framework methods. Every public function should be covered by at least one unit test. If a method has multiple branches every branch should be covered by a separate unit test. In some cases it might make sense to cover multiple branches in one unit test/test function, such a decision should however be made consciously.

### Integration tests

Integration tests are the second level or middle level of tests. These types of tests are used for example for module controllers. In such tests you should mock a request and test the response.

For large controllers it can make sense to define a `*ControllerTest` which uses `Traits` in order to categorize different suits of tests. For example in the `Admin` ApiControllerTest we implemented different traits for group, account and module tests.

### System tests

The system tests are the highest level of tests and test the overall functionality and if the implementation fulfills the specifications.

### Modules

Every module must implement the following tests if applicable:

* general module tests (e.g. install, update, delete, status change)
* admin tests (`use \Modules\tests\ModuleTestTrait;`)
* model tests (unit tests)
* controller tests (e.g. ApiController tests)
* view tests

You can find an example in the TestModule.

## Test documentation

* every test must have a short test description
* every test and description must be added to the test report
* every test needs a test category (e.g. framework, module etc.)
* every test should have a @covers annotation to specify which class it covers

## Tools

Tools used for the code inspection are:

* PHPUnit
* PHPStan
* Jasmine
* PHPCS
* Custom scripts/tools

These tools are all installed by running the `setup.sh` script from the Build repository.

### PHPUnit

This application uses PHPUnit as unit testing framework. Unit tests for specific classes need to be named in the same manner as the testing class.

In order to run all tests and also creating the dummy data for UI tests, execute the following command:

```sh
php vendor/bin/phpunit -c tests/PHPUnit/phpunit_no_coverage.xml
```

In order to also create a code coverage report run:

```sh
php -dxdebug.remote_enable=1 -dxdebug.mode=coverage,develop vendor/bin/phpunit -c tests/phpunit_default.xml
```

#### Modules

Every module needs to have a `Admin` directory containing a class called `AdminTest.php` which is used for testing the installation, activation, deactivation, uninstall and remove of the module. Tests that install, update, remove etc. a module need to have a group called `admin`. After running the `AdminTest.php` test the final state of the module should be installed and active, only this way it's possible to further test the controller and models. A code coverage of at least 90% is mandatory for every module for integration.

### PHPStan

With phpstan the code base is statically analyzed based on its configuration. This will help you to follow some of the "best" practices we enforce.

```sh
php vendor/bin/phpstan analyse --autoload-file=phpOMS/Autoloader.php -l 8 -c Build/Config/phpstan.neon --error-format=prettyJson ./ > Build/test/phpstan.json
```

### Jasmine

The javascript testing is done with jasmine. The javascript testing directory is structured the same way as the `Framework`. Unit tests for specific classes need to be named in the same manner as the testing class.

### PHP CS

Besides the code tests and static code analysis the code style is another very important inspection to ensure the code quality.

```sh
php vendor/bin/phpcs ./ --standard="Build/Config/phpcs.xml" -s --report-junit=Build/test/junit_phpcs.xml
```

### Custom scripts

#### Git Hooks (Linux only)

The git hooks perform various checks and validations during the `commit` and warn the developer about invalid code or code style/guideline violations.

For developers it is recommended to copy the contents of the `default.sh` file in the `Build` repository under `Hooks` to your `pre-commit` file in the `.git/hooks` directory. If the `pre-commit` file doesn't exist just create it.

The same should be done with every module. Simply go to `.git/modules/**/hooks` and also add the content of the `default.sh` file to all `pre-commit` files.

By doing this every commit will be inspected and either pass without warnings, pass with warnings or stop with errors. This will allow you to fix code before committing it. Be aware only changed files will be inspected. Also make sure all `pre-commit` files have `+x` permissions.

#### Release Report

The (TestReportGenerator)[https://github.com/Karaka-Management/TestReportGenerator] generates a customer report which outputs various information regarding tests (unit, integration, static) and code quality. The primary purpose of this report generator is to aggregate the code inspections in a printable format that can be used for auditing purposes on the customer side.

```sh
php TestReportGenerator/src/index.php \
    -b /home/oms \
    -l /home/oms/Build/Config/reportLang.php \
    -c /home/oms/tests/coverage.xml \
    -s /home/oms/Build/test/junit_phpcs.xml \
    -sj /home/oms/Build/test/junit_eslint.xml \
    -a /home/oms/Build/test/phpstan.json \
    -u /home/oms/Build/test/junit_php.xml \
    -d /home/oms/Build/test/ReportExternal \
    --version 1.0.0
```

## Demo setup

A good way to setup a demo application with mostly randomly generated user input data is the **demoSetup** script which can be found in the repository https://github.com/Karaka-Management/demoSetup.

The following command will create a demo application:

```sh
php demoSetup/setup.php
```

In some cases code changes may require changes to the demo setup script (e.g. changes in the api, new modules). Since the demo setup script tries to simulate user generated data it takes some time to run. You may speed up the runtime by parallelizing the execution. However, this may use up 100% of your CPU and storage performance.

```sh
php demoSetup/setup.php -a 0
```
