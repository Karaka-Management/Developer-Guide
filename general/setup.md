# Install

In order to setup the application for development for the first time please see the following instructions and recommendations.

## Dev-Environment Requirements

Make sure your dev-environment or server fulfills the following requirements:

* PHP >= 8.0
* PHP extensions: mbstring, gd, zip, dom, pdo, pdo-mysql/pdo-pgsql/pdo-sqlsrv, sqlite, bcmath, imap\*, redis\*, memcached\*, ftp\*, socket\*, curl\*, xml\*
  * Extension list: `php8.0 php8.0-dev php8.0-cli php8.0-common php8.0-mysql php8.0-pgsql php8.0-xdebug php8.0-opcache php8.0-pdo php8.0-sqlite php8.0-mbstring php8.0-curl php8.0-imap php8.0-bcmath php8.0-zip php8.0-dom php8.0-xml php8.0-phar php8.0-gd php-pear`
* databases: mysql/postgresql/sqlsrv
* web server: apache2/nginx
    * mod_headers (apache2)
* software: tesseract-ocr\*, pdftotext\*, pdftoppm\*

The application and frameworks can use different databases. For the normal development process you only need one (whichever you prefer). However, in order to test against all supported databases and all code paths you would have to install all above mentioned databases.

Extensions and software marked with `*` are optional but recommended. They are only required in special situations. Requirements with a `/` in between mean that only one of the dependencies is necessary depending on your preferences and previous decisions.

Steps which are not explained in this documentation are how to install and setup the above mentioned software and extensions. You also should configure the web server paths accordingly in order to access the application in the browser.

### IDE / Editor

Which IDE or editor a developer uses is up to the individual developer. From experience the following opinionated choices provided good results:

* Visual Studio Code (vscode)
* Sublime
* PHPStorm (mostly for php, html, css, js)

### Installation Options

1. Option 1: Full installation, code checks/tests, generating documentation. **Not recomended as quick setup**
2. Option 2: Only installs the application with some tests. Requires you to install the dev tools manually. **Recommended**
3. Option 3: Only installs the application, due to the large amount of data takes some time to execute. **Recommended**

## Option 1: Linux Shell Script

This also installs all required dev tools and sets up the directory structure by itself. Using this method also tears down previous installs for a fresh install perfect for re-installing from the current development version. Furthermore, the use of PHPUnit also makes sure that the application is working as intended. The PHPUnit install also provides lots of dummy data for better integration and functionality testing of your own code/modules.

### Steps

The following steps will setup the application, download all necessary tools and perform extensive code quality checks and documentation tasks:

1. Go to the directory where you want to install the build script
2. Run `git clone -b develop https://github.com/karaka-management/Build.git`
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
2. Run `git clone -b develop https://github.com/karaka-management/Karaka.git`
3. Run `git submodule update --init --recursive`
4. Run `git submodule foreach git checkout develop`
5. Install Composer
6. Run `composer install` inside `Karaka`
7. Run `php vendor/bin/phpunit --configuration tests/phpunit_no_coverage.xml` inside `Karaka` or open `http://127.0.0.1/Install`

After the installation you'll have access to the following content:

* Application: `http://127.0.0.1`
* Code Coverager: `http://127.0.0.1/Build/coverage/`

### Annotation

During this process the database automatically gets dropped (if it exists) and re-created. If you don't have `xdebug` installed but `phpdbg` you can replace `php phpunit ...` with `phpdbg -qrr phpunit.phar ...` or use `pcov` for much faster code coverage generation.

## Option 3: Demo Application

This will only setup the application including some dummy data and also perform the code tests but no quality checks. Compared to option 2 this includes much more test data and it doesn't execute a unit test.

1. Go to the directory where you want to install the application
2. Run `git clone -b develop https://github.com/karaka-management/Karaka.git`
3. Run `git submodule update --init --recursive`
4. Run `git submodule foreach git checkout develop`
5. Install Composer
6. Run `composer install` inside `Karaka`
7. Create the database table `oms`
7. Run `php demoSetup/setup.php` inside `Karaka`

After the installation you'll have access to the following content:

* Application: `http://127.0.0.1`

### Annotation

* During this process the database automatically gets dropped (if it exists) and re-created
* The total storage space needed for this installation (w/o profiling data) is ≈10 GB
* The database space needed for this installation is ≈500 MB
* The runtime of the script is approx. 1h but can be slower depending on the hardware (slower CPU, hard drive vs SSD)

### Profiling

You maybe want to use the following command to run the install script instead in order to also generate a cachegrind output for memory and performance profiling:

```sh
php -dxdebug.profiler_enable=1 -dxdebug.mode=develop,debug,profile -dxdebug.output_dir=/your/path demoSetup/setup.php
```

> This may use a lot of resources and storage space (≈15 GB of cachegrind data w/o trace data and ≈120 GB w/ trace data)

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

## cOMS

### OpenCV

https://docs.opencv.org/3.4/d7/d9f/tutorial_linux_install.html

