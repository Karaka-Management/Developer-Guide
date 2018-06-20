# Installation

Installing the application as a developer can be achived by following one of the following instructions.

## Server Requirements

* PHP >= 7.2
* PDO PHP Extension

### Recommended Extensions

* Memcache
* Sqlite
* Socket
* Curl
* Imap
* bcmath
* zip
* mbstring

## Linux Shell Script

This is the prefered way to install the application since this also installs all required dev tools and sets up the direcetory structure by itself. Using this method also tears down previous installs for a fresh install perfect for re-installing from the current development version. Furthermore the use of phpUnit also makes sure that the application is working as intended. The phpUnit install also provides lots of dummy data for better integration and functionality testing of your own code/modules.

### Steps

The following steps will setup the application and perform extensive code quality checks and documentation tasks:

1. Go somewhere where you want to install the build script
2. Enter `git clone -b develop https://github.com/Orange-Management/Build.git`
3. Modify `config.sh`
4. Run `chmod 777 buildProject.sh`
5. Run `./buildProject.sh`

Alternatively after cloning the git repository:

1. Run `php phpunit.phar --configuration tests/phpunit_no_coverage.xml` inside `Orange-Management` or open `http://your_url.com/Install`

This wil only setup the application instead of also performing additional code quality checks.

### Annotation

During this process the database automatically gets dropped (if existing) and re-created. The database user and password can't be changed right now since the install config relies on the same data. Future releases will make use of a new user that will get set up by the install script as well. If you don't have `xdebug` installed but `phpdbg` you can replace `php phpunit.phar ...` with `phpdbg -qrr phpunit.phar ...`.
