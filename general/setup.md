# Install

In order to setup the application for development for the first time please see the following instructions and recommendations.

## Dev-Environment Requirements

Follow the general install/setup documentation until the application setup: [Install](https://github.com/Karaka-Management/Documentation/blob/develop/setup/install.md)

The following dev tools are highly recommended and the documentation assumes you installed them and added them to your environment paths on Windows. On linux they should be accessible immediately after installation.

* [git](https://git-scm.com/)
* [composer](https://getcomposer.org/)
* [npm](https://docs.npmjs.com/)

### IDE / Editor

Which IDE or editor a developer uses is up to the individual developer. From experience the following opinionated choices provided good results:

* [Visual Studio Code](https://code.visualstudio.com/)
* [Sublime](https://www.sublimetext.com/)
* [PhpStorm](https://www.jetbrains.com/phpstorm/) (mostly for php, html, css, js)

## Application install options

Option 1 and 2 require you to install the dev tools in advance!

1. Option: Installs the application (with a lot of dummy data, this may take a long time). **Recommended**
2. Option: Installs the application (with or without performing tests). **Recommended**
3. Option: Full installation, code checks/tests, generating documentation. **Not recomended**

### Option 1: Demo Application

This will only setup the application including some dummy data and also perform the code tests but no quality checks. Compared to option 2 this includes much more test data and it doesn't execute a unit test.

1. Go to the directory where you want to install the application
2. Run `git clone -b develop https://github.com/karaka-management/Karaka.git`
3. Run `git submodule update --init --recursive`
4. Run `git submodule foreach git checkout develop`
5. Run `git clone -b master https://github.com/Karaka-Management/demoSetup.git`
6. Run `composer install` inside `Karaka`
7. Run `npm install` inside `Karaka`
8. Create the database `oms` in your database management software
9. Adjust the `demoSetup/config.php` file according to your settings (e.g. database user name + password)
10. Run `php demoSetup/setup.php` inside `Karaka` (takes a long time: > 1h)

After the installation you'll have access to the following content:

* Application: `http://127.0.0.1`

Instead of calling `php demoSetup/setup.php` which only uses a single thread you may also run `php demoSetup/setup.php -a 0` which will execute the install script in multiple threads leading to most likely 100% CPU and storage usage but therfore significantly reduce the execution time.

> You might want to call the setup script as a different user to ensure the same permissions `sudo -u wwww-data php demoSetup/setup.php`

#### Annotation

* During this process the database automatically gets dropped (if it exists) and re-created
* The total storage space needed for this installation (w/o profiling data) is ≈10 GB
* The database space needed for this installation is ≈500 MB
* The runtime of the script is approx. 1h but can be slower depending on the hardware (slower CPU, hard drive vs SSD)

#### Profiling

You maybe want to use the following command to run the install script instead in order to also generate a cachegrind output for memory and performance profiling:

```sh
php -dxdebug.profiler_enable=1 -dxdebug.mode=develop,debug,profile -dxdebug.output_dir=/your/path demoSetup/setup.php
```

> This may use a lot of resources and storage space (≈15 GB of cachegrind data w/o trace data and ≈120 GB w/ trace data)

### Option 2: PHPUnit Test Suits

This will only setup the application including some dummy data and also perform the code tests but no quality checks.

#### Steps

1. Go to the directory where you want to install the application
2. Run `git clone -b develop https://github.com/karaka-management/Karaka.git`
3. Run `git submodule update --init --recursive`
4. Run `git submodule foreach git checkout develop`
5. Run `composer install` inside `Karaka`
6. Run `npm install` inside `Karaka`
7. Create the database `oms` in your database management software
8. Adjust the `tests/Bootstrap.php` file according to your settings (e.g. database user name + password)
9. Run `php vendor/bin/phpunit --configuration tests/phpunit_no_coverage.xml` inside `Karaka` or open `http://127.0.0.1/Install`

After the installation you'll have access to the following content:

* Application: `http://127.0.0.1`
* Code Coverager: `http://127.0.0.1/Build/coverage/`

> During this process the database automatically gets dropped (if it exists) and re-created. If you don't have `xdebug` installed but `phpdbg` you can replace `php phpunit ...` with `phpdbg -qrr phpunit.phar ...` or use `pcov` for much faster code coverage generation.

### Option 3: Linux Shell Script

This also installs all required dev tools and sets up the directory structure by itself. Using this method also tears down previous installs for a fresh install perfect for re-installing from the current development version. Furthermore, the use of PHPUnit also makes sure that the application is working as intended. The PHPUnit install also provides lots of dummy data for better integration and functionality testing of your own code/modules.

#### Steps

The following steps will setup the application, download all necessary tools and perform extensive code quality checks and documentation tasks:

1. Go to the directory where you want to install the build script (this is not the place where you want to install the application)
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

> During this process the database automatically gets dropped (if it exists) and re-created. If you don't have `xdebug` installed but `phpdbg` you can replace `php phpunit ...` with `phpdbg -qrr phpunit.phar ...` or use `pcov` for much faster code coverage generation in `Build/Inspection/Php/tests.sh`
>
## cOMS

If you are interest on working on the c++ code base you will in addition need the following tools and libraries:

* [gcc](https://gcc.gnu.org/) version 7.5.0 or newer
* [cmake](https://cmake.org/)
* [OpenCV](https://docs.opencv.org/3.4/d7/d9f/tutorial_linux_install.html)
