# Install

In order to setup the application for development for the first time please see the following instructions and recommendations.

* [Dev-Environment Requirements](#dev-environment-requirements)
* [Application install options](#application-install-options)
* [cOMS](#coms)

## Dev-Environment Requirements

Follow the general install/setup documentation until the application setup: [Install](https://github.com/Karaka-Management/User-Guide/blob/develop/setup/install.md)

The following dev tools are highly recommended and the documentation assumes you installed them and added them to your environment paths on Windows. On linux they should be accessible immediately after installation.

### Quick software overview

```sh
# For php/html/javascript developers
sudo apt-get install git poppler-utils mariadb-server mariadb-client postgresql postgresql-contrib vsftpd tesseract-ocr wget curl grep sed composer nodejs npm software-properties-common php8.1 php8.1-dev php8.1-cli php8.1-common php8.1-mysql php8.1-pgsql php8.1-xdebug php8.1-opcache php8.1-pdo php8.1-sqlite php8.1-mbstring php8.1-curl php8.1-imap php8.1-bcmath php8.1-zip php8.1-dom php8.1-xml php8.1-phar php8.1-gd php-pear apache2 redis redis-server memcached sqlite3 wkhtmltopdf

sudo systemctl enable apache2
sudo mysql_secure_installation
sudo systemctl start mariadb
sudo systemctl enable mariadb

sudo a2enmod rewrite
sudo a2enmod expires
sudo a2enmod headers
sudo systemctl restart apache2

# For c/c++ developers (additional may be required, see below)
sudo apt-get install gcc curl libcurl4-openssl-dev libxml2 libxml2-dev cmake
```

### IDE / Editor

Which IDE or editor a developer uses is up to the individual developer. From experience the following opinionated choices provided good results:

* [Visual Studio Code](https://code.visualstudio.com/)
* [Sublime](https://www.sublimetext.com/)
* [PhpStorm](https://www.jetbrains.com/phpstorm/) (mostly for php, html, css, js)

## Application install options

Option 1 and 2 require you to install the dev tools in advance!

1. Option: Use the virtual machine we provide for devs which has everything setup and configured to start almost instantly after download **Most Recommended**
2. Option: Installs the application (with a lot of dummy data, this may take a long time). **Recommended (slow but a lot of useful data)**
3. Option: Installs the application (with or without performing tests). **Recommended (slow and much less useful data)**

### Option 1: VM

Please contact us if you would like to use our VM, we will send you a download link.

1. Install VirtualBox

2. Download the VM image

3. Setup your IDE (i.e. to use ssh, or mount the git repository, ...)
   1. Username: jingga
   2. Password: orange


> The VM has apache2 and a database already running with dummy data for immediate development. The VM size is 60 GB.

Additional tools and settings coming with the VM:

1. Automatic trace and benchmark generation with every web request in `/var/www/html/webgrind/Logs`
2. Webgrind view `http://vm_ip:82`
3. Trace visualization `http://vm_ip:81`
   1. Download the latest trace from `http://vm_ip:82/Logs`
   2. Drag and drop that downloaded `*.xt` file in the trace visualizer
4. sitespeed.io `http://vm_ip:83`

### Option 2: Demo Application

This will only setup the application including some dummy data and also perform the code tests but no quality checks. Compared to option 2 this includes much more test data and it doesn't execute a unit test.

1. Go to the directory where you want to install the application
2. Run `git clone -b develop --recurse-submodules -j8 https://github.com/Karaka-Management/Karaka.git`
3. Run `git submodule foreach git checkout develop`
4. Run `git clone -b master https://github.com/Karaka-Management/demoSetup.git`
5. Run `composer install` inside `Karaka`
6. Run `npm install` inside `Karaka`
7. Create the database `omd` in your database management software
8. Adjust the `demoSetup/config.php` file according to your settings (e.g. database user name + password)
9. Run `php demoSetup/setup.php` inside `Karaka` (takes a long time: > 1h)

After the installation you'll have access to the following content:

* Application: `http://127.0.0.1`

Instead of calling `php demoSetup/setup.php` which only uses a single thread you may also run `php demoSetup/setup.php -a 0` which will execute the install script in multiple threads leading to most likely 100% CPU and storage usage but therfore significantly reduce the execution time.

> You may want to call the setup script as a different user to ensure the same permissions `sudo -u wwww-data php demoSetup/setup.php`

> For debugging you may use the command `sudo -u www-data php -dxdebug.mode=debug -dxdebug.start_with_request=yes demoSetup/setup.php`

#### Annotation

* During this process the database automatically gets dropped (if it exists) and re-created
* The total storage space needed for this installation (w/o profiling data) is ≈10 GB
* The database space needed for this installation is ≈500 MB
* The runtime of the script is approx. 1h but can be slower depending on the hardware (slower CPU, hard drive vs SSD)

#### Profiling

You may want to use the following command to run the install script instead in order to also generate a cachegrind output for memory and performance profiling:

```sh
php -dxdebug.mode=develop,debug,profile -dxdebug.start_with_request=yes -dxdebug.output_dir=/your/path demoSetup/setup.php
```

> This may use a lot of resources and storage space (≈15 GB of cachegrind data w/o trace data and ≈120 GB w/ trace data)

### Option 3: PHPUnit Test Suits

This will only setup the application including some dummy data and also perform the code tests but no quality checks.

#### Steps

1. Go to the directory where you want to install the application
2. Run `git clone -b develop --recurse-submodules -j8 https://github.com/Karaka-Management/Karaka.git`
3. Run `git submodule foreach git checkout develop`
4. Run `composer install` inside `Karaka`
5. Run `npm install` inside `Karaka`
6. Create the database `oms` in your database management software
7. Adjust the `tests/Bootstrap.php` file according to your settings (e.g. database user name + password)
8. Run `php -dxdebug.mode=develop,debug,profile,coverage -dxdebug.start_with_request=yes vendor/bin/phpunit --configuration tests/phpunit_default.xml` inside `Karaka` or open `http://127.0.0.1/Install` for a web install without dummy data.

After the installation you'll have access to the following content:

* Application: `http://127.0.0.1`
* Code Coverager: `http://127.0.0.1/tests/coverage/`

> During this process the database automatically gets dropped (if it exists) and re-created. If you don't have `xdebug` installed but `phpdbg` you can replace `php phpunit ...` with `phpdbg -qrr phpunit.phar ...` or use `pcov` for much faster code coverage generation.

## cOMS

If you are interest on working on the c++ code base you will in addition need the following tools and libraries:

* [OpenCV](https://docs.opencv.org/3.4/d7/d9f/tutorial_linux_install.html)
