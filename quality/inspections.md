# Code Inspections & Tests

Code inspections are very important in order to maintain the same code quality throughout the application. The [Build](https://github.com/Karaka-Management/Build) repository and package managers such as `composer` and `npm` contain all essential configuration files for the respective php and javascript inspection tools. The framework and every module will be evaluated based on the defined code and quality standards. Only code that passes all code, quality and test standards is accepted. Updates and bug fixes also must follow these standards.

## Summary

The following automated tests must pass without errors, failures and warnings for successful code changes:

* `php ./vendor/bin/phpstan analyse -l 9 -c Build/Config/phpstan.neon ./`
* `php ./vendor/bin/php-cs-fixer fix ./ --config=Build/Config/.php-cs-fixer.php --allow-risky=yes`
* `php ./vendor/bin/phpcbf --standard=Build/Config/phpcs.xml ./`
* `php ./vendor/bin/phpcs --severity=1 ./ --standard="Build/Config/phpcs.xml"`
* `php ./vendor/bin/rector process --config Build/Config/rector.php --dry-run ./`
* `npx eslint ./ -c Build/Config/.eslintrc.json`
* `find . -regex '.*\.\(cpp\|h\)' -exec clang-format -style=file:Build/Config/.clang-format -i {} \;`
* `php ./vendor/bin/phpunit -c tests/phpunit_no_coverage.xml`
* `./Build/Config/jasmine_build.sh && npx jasmine --config=Build/Config/jasmine.json`
* `./cOMS/tests/test.sh`

<p class="cT">
<img width="150px" tabindex="0" src="./Developer-Guide/quality/img/webgrind.jpg"> <img width="150px" tabindex="0" src="./Developer-Guide/quality/img/trace_visualizer.jpg"> <img width="150px" tabindex="0" src="./Developer-Guide/quality/img/sitespeed.jpg"> <img width="150px" tabindex="0" src="./Developer-Guide/quality/img/sitespeed_waterfall.jpg"> <img width="150px" tabindex="0" src="./Developer-Guide/quality/img/codecoverage.jpg"> <img width="150px" tabindex="0" src="./Developer-Guide/quality/img/coverage_analysis.jpg"> <img width="150px" tabindex="0" src="./Developer-Guide/quality/img/metrics.jpg"> <img width="150px" tabindex="0" src="./Developer-Guide/quality/img/phpunit_html.jpg"> <img width="150px" tabindex="0" src="./Developer-Guide/quality/img/phpcs.jpg"> <img width="150px" tabindex="0" src="./Developer-Guide/quality/img/phpstan.jpg"> <img width="150px" tabindex="0" src="./Developer-Guide/quality/img/rector.jpg">
</p>

Alternatively you can simply run the helper script in the Build repository, which executes a few of the above mentioned checks:

```sh
./Build/Helper/inspectproject.sh
```

An overview of all tests and statistics can be found at https://dev.jingga.app. Navigate to the `./src` directory and then open the `./src/**/build` directory of the respective repository.

> Manual tests and inspections may reveal further issues during the review process requiring additional code changes

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

### Test locations

* Application: `tests`
* Php framework: `phpOMS/tests`
* Modules: `Modules/**/tests`
* Js framework: `jsOMS/tests`
* C++ framework + tools: `cOMS/tests`
* Css framework: `cssOMS/tests`
* Configurations for tools: `Build/Config`
* Inspection script: `Build/php.sh` or `Build/Helper/Scripts/inspectproject.sh`
* End-to-End: `tests/Web`

### Unit tests

The smallest/lowest level of testing are the unit tests. Unit tests should be implemented for models and framework methods. Every public function should be covered by at least one unit test. If a method has multiple branches every branch should be covered by a separate unit test. In some cases it might make sense to cover multiple branches in one unit test/test function, such a decision should however be made consciously.

### Integration tests

Integration tests are the second level or middle level of tests. These types of tests are used for example for module controllers. In such tests you should mock a request and test the response.

For large controllers it can make sense to define a `*ControllerTest` which uses `Traits` in order to categorize different suits of tests. For example in the `Admin` ApiControllerTest we implemented different traits for group, account and module tests.

### User/End-To-End

The User/End-To-End tests are the highest level of tests and test the overall functionality and if the implementation fulfills the specifications.

### Modules

Every module must implement the following tests if applicable:

* general module tests (e.g. `AdminTest.php`, install, update, delete, status change)
* admin tests (`use \tests\Modules\ModuleTestTrait;`)
* model tests (unit tests)
* controller tests (e.g. ApiController tests)
* view tests

You can find an example in the TestModule.

### Demo test

The demo application is a simple way to check the overall functionality of the application and provide a user with a large amount of data for manual tests.

A good way to setup a demo application with mostly randomly generated user input data is the **demoSetup** script which can be found in the repository [demoSetup](https://github.com/Karaka-Management/demoSetup).

The following command will create a demo application:

```sh
php demoSetup/setup.php
```

> You might want to call the setup script as a different user to ensure the same permissions `sudo -u wwww-demo php demoSetup/setup.php` or `sudo -u wwww-data php demoSetup/setup.php`

In some cases code changes may require changes to the demo setup script (e.g. changes in the api, new modules). Since the demo setup script tries to simulate user generated data it takes some time to run. You may speed up the runtime by parallelizing the execution. However, this may use up 100% of your CPU and storage performance:

```sh
php demoSetup/setup.php -a 0
```

### End-To-End tests

End-To-End tests simulate the user interaction with the application in the browser. These tests can be found in `tests/Web` and can only be run on a machine with `Chrome` and a window manager installed (these tests don't run in a pure CLI environment).

```sh
node test/Web/Backend/Install.js
```

### UI tests

While UI tests can be part of unit, integration or system tests the `cssOMS` repository also includes a simple test suit at [http://127.0.0.1/cssOMS/tests/app](http://127.0.0.1/cssOMS/tests/app) which allows developers to test UI elements and check how they work.

#### Live debugging

In the demo application it is possible to highlight html and css warnings (e.g. missing attributes, deprecated tags, inline styles, ...). In order to activate the live debugging add `&debug=true` to the end of your url.

## Test documentation

* every test must have a short test description
* every test and description must be added to the test report
* every test needs a test category (for PHPUnit tests) (e.g. framework, module etc.)
* every test should have a covers attribute (for PHPUnit tests) to specify which class it covers

## Tools

Tools used for the code inspection are:

* PHPUnit
* PHPStan
* Jasmine
* PHPCS
* PHP CS Fixer
* PHP CBF
* Rector
* Clang-Format
* Custom scripts/tools

### PHPUnit

This application uses PHPUnit as unit testing framework. Unit tests for specific classes need to be named in the same manner as the testing class.

In order to run all tests and also creating the dummy data for UI tests, execute the following command:

```sh
php vendor/bin/phpunit -c tests/PHPUnit/phpunit_no_coverage.xml
```

In order to also create a code coverage report run:

```sh
php -dxdebug.mode=coverage,develop,debug vendor/bin/phpunit -c tests/phpunit_default.xml --log-junit Build/test/junit_php.xml --coverage-html Build/coverage
```

A visualization of the coverage can be found at http://127.0.0.1/Build/coverage

If you would like to run a individual test suit run:

```sh
php -dxdebug.remote_enable=1 -dxdebug.start_with_request=yes -dxdebug.mode=coverage,develop,debug vendor/bin/phpunit tests/MyTest.php

```

#### Modules

Every module needs to have a `Admin` directory containing a class called `AdminTest.php` which is used for testing the installation, activation, deactivation, uninstall and remove of the module. Tests that install, update, remove etc. a module need to have a group called `admin`. After running the `AdminTest.php` test the final state of the module should be installed and active, only this way it's possible to further test the controller and models. A code coverage of at least 90% is mandatory for every module for integration.

### PHPmetrics

While phpmetrics doesn't provide additional tests it is useful to visualize dependencies and potentially critical code. Including the junit report `junit_php.xml` in the command below requires you to run the PHPUnit tests first.

```sh
php vendor/bin/phpmetrics --config=Build/Config/phpmetrics.json --junit=Build/test/junit_php.xml --report-html=Build/test/metrics
```

A visualization of the metrics can be found at http://127.0.0.1/Build/test/metrics

### PHPStan

With phpstan the code base is statically analyzed based on its configuration. This will help you to follow some of the "best" practices we enforce.

```sh
php vendor/bin/phpstan analyse -l 9 -c Build/Config/phpstan.neon --error-format=prettyJson ./ > Build/test/phpstan.json
```

### PHP CS

The php code base has a defined code style standard. The easiest way to check for most rules is to run phpcs.

```sh
php vendor/bin/phpcs --severity=1 ./ --standard="Build/Config/phpcs.xml" -s --report-junit=Build/test/junit_phpcs.xml
```

> Many IDEs allow to integrate phpcs rules/configuration files for automatic checks in the editor

### PHP CS Fixer

The php code base has a defined code style standard. The following command automatically fixes some of the violations

```sh
php vendor/bin/php-cs-fixer fix ./ --config=Build/Config/.php-cs-fixer.php --allow-risky=yes
```

### PHP CBF

The php code base has a defined code style standard. The following command automatically fixes some of the violations

```sh
php vendor/bin/phpcbf --standard=Build/Config/phpcs.xml ./
```

### Rector

```sh
php vendor/bin/rector process --dry-run --config Build/Config/rector.php ./
```

### Clang-Format

The c/c++ code base has a defined code style standard. The following command automatically fixes some of the violations

```sh
find . -regex '.*\.\(cpp\|h\)' -exec clang-format -style=file:Build/Config/.clang-format -i {} \;
```

### Jasmine

The javascript testing is done with jasmine. The javascript testing directory is structured the same way as the `Framework`. Unit tests for specific classes need to be named in the same manner as the testing class.

```sh
./Build/Config/jasmine_build.sh && npx jasmine --config=Build/Config/jasmine.json
```

Alternatively you can open `http://127.0.0.1/jsOMS/tests/SpecRunner.html` to run the tests in your browser.

### JS Eslint

The js code base has a defined code style standard. The easiest way to check for most rules is to run eslint.

```sh
npx eslint ./ -c Build/Config/.eslintrc.json
```

> Many IDEs allow to integrate eslint rules/configuration files for automatic checks in the editor

### Sitespeed

You can perform sitespeed checks by using sitespeed.io. Example:

```sh
sitespeed.io ./Build/Helper/Scripts/sitespeedDemoUrls.txt -b chrome --outputFolder /var/www/html/sitespeed
```

### Profiles & trace views

1. Automatic trace and benchmark generation with every web request in `/var/www/html/webgrind/Logs`
2. Webgrind view `http://vm_ip:82`
3. Trace visualization `http://vm_ip:81`
   1. Download the latest trace from `http://vm_ip:82/Logs`
   2. Drag and drop that downloaded `*.xt` file in the trace visualizer

### SQL performance

With query profiling enabled you can inspect slow running queries that may need optimization. The threshold for slow running queries is defined at 0.5s.

```sh
mysqldumpslow -t 10 /var/log/mysql/mysql-slow.log
mysqldumpslow -t 10 -s l /var/log/mysql/mysql-slow.log
```

### Custom scripts

#### C++ tests

The c++ tests use the internal assertion/testing functions. You can run all tests by running the test script.

```sh
./cOMS/tests/test.sh
```

#### Git Hooks (Linux only)

The git hooks perform various checks and validations during the `commit` and warn the developer about invalid code or code style/guideline violations. However, the checks and validations performed are only those which can be done very quickly, to avoid commit slow downs.

For developers it is recommended to copy the contents of the `default.sh` file in the `Build` repository under `Hooks` to your `pre-commit` file in the `.git/hooks` directory. If the `pre-commit` file doesn't exist just create it.

The same should be done with every module. Simply go to `.git/modules/**/hooks` and also add the content of the `default.sh` file to all `pre-commit` files.

By doing this every commit will be inspected and either pass without warnings, pass with warnings or stop with errors. This will allow you to fix code before committing it. Be aware only changed files will be inspected. Also make sure all `pre-commit` files have `+x` permissions.

#### Release Report

The [TestReportGenerator](https://github.com/Karaka-Management/TestReportGenerator) generates a customer report which outputs various information regarding tests (unit, integration, static) and code quality. The primary purpose of this report generator is to aggregate the code inspections in a printable format that can be used for auditing purposes on the customer side.

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

#### Other checks

The following checks should also be performed. If you use the git hooks from the Build repository, they are automatically done during the staging process.

* Missing alt for images
* Missing type in input
* Missing name in form elements
* No inline style
* Hard coded text in localizable attributes
* Hard coded text in localizable tag
* Php files without strict_types
* Has logs
* Has whitespace at line end

## Classification

```mermaid
quadrantChart;
    x-axis Low Level --> High Level;
    y-axis Low Importance --> High Importance;
    PHPStan: [0.2, 0.5];
    *UI tests: [0.8, 0.05];
    Code Style: [0.1, 0.1];
    PHPUnit: [0.5, 0.75];
    cOMS/tests.sh: [0.2, 0.2];
    Jasmine: [0.3, 0.5];
    *Selenium: [0.9, 0.9];
    *Sitespeed: [0.75, 0.25];
    *Demo: [0.95, 0.95];
    *Other: [0.3, 0.05];
```

\* Optional and/or manual

## References

[Development process](https://github.com/Karaka-Management/Organization-Guide/blob/master/Processes/01_Development.md)
