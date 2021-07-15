# Setup

In order to setup the application for development for the first time please see the following instructions and recommendations.

## Dev-Environment Requirements

Make sure your dev-environment or server fulfills the following requirements:

* PHP >= 8.0
* PHP extensions: mbstring, gd, zip, dom, mysql/pgsql/sqlsrv, sqlite, bcmath, imap\*, redis\*, memcached\*, ftp\*, socket\*, curl\*, xml\*
* databases: mysql, postgresql, sqlsrv
* webserver: apache2
    * mod_headers

The application and frameworks can use different databases. For the normal development process you only need one (whichever you prefer). However, in order to test against all supported databases and all code paths you would have to install all above mentioned databases.

Extensions marked with `*` are optional. They are only required in special situations.

Steps which are not explained in this documentation are how to install and setup the above mentioned software and extensions. You also should configure the webserver paths accordingly in order to access the application in the browser.

## Option 1: Linux Shell Script

This also installs all required dev tools and sets up the directory structure by itself. Using this method also tears down previous installs for a fresh install perfect for re-installing from the current development version. Furthermore, the use of PHPUnit also makes sure that the application is working as intended. The PHPUnit install also provides lots of dummy data for better integration and functionality testing of your own code/modules.

### Steps

The following steps will setup the application, download all necessary tools and perform extensive code quality checks and documentation tasks:

1. Go to the directory where you want to install the build script
2. Run `git clone -b develop https://github.com/Orange-Management/Build.git`
3. Modify `config.sh` (most likely the db credentials and paths)
4. Run `chmod +x buildProject.sh`
5. Run `./buildProject.sh`

After the installation you'll have access to the following content:

* Application: `http://127.0.0.1`
* Code Coverage: `http://127.0.0.1/Build/coverage/`
* Test Report: `${INSPECTION_PATH}/test/ReportExternal`
* Unit Test Report: `${INSPECTION_PATH}/test/testdox.txt`
* PHPStan Report: `${INSPECTION_PATH}/test/phpstan.json`
* ... and more

### Annotation

During this process the database automatically gets dropped (if it exists) and re-created. If you don't have `xdebug` installed but `phpdbg` you can replace `php phpunit ...` with `phpdbg -qrr phpunit.phar ...` or use `pcov` for much faster code coverage generation in `Build/Inspection/Php/tests.sh`

## Option 2: PHPUnit Test Suits

This will only setup the application including some dummy data and also perform the code tests but no quality checks.

### Steps

1. Go to the directory where you want to install the application
2. Run `git clone -b develop https://github.com/Orange-Management/Orange-Management.git`
3. Run `git submodule update --init --recursive >/dev/null`
4. Run `git submodule foreach git checkout develop >/dev/null`
5. Install Composer
6. Run `composer install` inside `Orange-Management`
7. Run `php vendor/bin/phpunit --configuration tests/phpunit_no_coverage.xml` inside `Orange-Management` or open `http://127.0.0.1/Install`

After the installation you'll have access to the following content:

* Application: `http://127.0.0.1`
* Code Coverager: `http://127.0.0.1/Build/coverage/`

### Annotation

During this process the database automatically gets dropped (if it exists) and re-created. If you don't have `xdebug` installed but `phpdbg` you can replace `php phpunit ...` with `phpdbg -qrr phpunit.phar ...` or use `pcov` for much faster code coverage generation.

## Git Hooks (Linux only)

The git hooks perform various checks and validations during the `commit` and warn the developer about invalid code or code style/guideline violations.

For developers it is recommended to copy the contents of the `default.sh` file in the `Build` repository under `Hooks` to your `pre-commit` file in the `.git/hooks` directory. If the `pre-commit` file doesn't exist just create it.

The same should be done with every module. Simply go to `.git/modules/**/hooks` and also add the content of the `default.sh` file to all `pre-commit` files.

By doing this every commit will be inspected and either pass without warnings, pass with warnings or stop with errors. This will allow you to fix code before committing it. Be aware only changed files will be inspected. Also make sure all `pre-commit` have `+x` permissions.

## Tools

The following tools are important to test the application and to ensure the code quality. The configurations and sample shell executions can be found in the `Build` directory. These tools are also downloaded during the setup process of the `buildProject.sh` script.

* composer
* phploc
* phpunit
* phpcs
* phpmetrics
* documentor
* phpstan