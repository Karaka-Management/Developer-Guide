# Installation

Installing the application as a developer can be achived by following one of the following instructions.

## Server Requirements

* PHP >= 7.2
* PDO PHP Extension
* mbstring
* database such as mysql
* apache2
    * mod_headers

### Recommended Extensions

The following extensions are recommended for tests and in some cases required by the build tools

* ast
* phear
* memcache
* Sqlite
* socket
* curl
* imap
* bcmath
* zip
* php*-dev
* dom
* xml
* phar
* opcache
* gd / gd2

## Linux Shell Script

This is the preferred way to install the application since this also installs all required dev tools and sets up the directory structure by itself. Using this method also tears down previous installs for a fresh install perfect for re-installing from the current development version. Furthermore the use of phpUnit also makes sure that the application is working as intended. The phpUnit install also provides lots of dummy data for better integration and functionality testing of your own code/modules.

### Steps

The following steps will setup the application, download all necessary tools and perform extensive code quality checks and documentation tasks:

1. Go somewhere where you want to install the build script
2. Enter `git clone -b develop https://github.com/Orange-Management/Build.git`
3. Modify `config.sh`
4. Run `chmod +x buildProject.sh`
5. Run `./buildProject.sh`

Alternatively after cloning the git repository:

1. Run `php composor.phar install` inside `Orange-Management`
2. Run `php phpunit.phar --configuration tests/phpunit_no_coverage.xml` inside `Orange-Management` or open `http://your_url.com/Install`

This wil only setup the application instead of also performing additional code quality checks.

### Annotation

During this process the database automatically gets dropped (if existing) and re-created. The database user and password can't be changed right now since the install config relies on the same data. Future releases will make use of a new user that will get set up by the install script as well. If you don't have `xdebug` installed but `phpdbg` you can replace `php phpunit.phar ...` with `phpdbg -qrr phpunit.phar ...`.

## Git Hooks (Linux only)

For developers it is recommended to copy the contents of the `default.sh` file in the `Build` repository under `Hooks` to your `pre-commit` file in the `.git/hooks` directory. If the `pre-commit` file doesn't exist just create it.

The same should be done with every module. Simply go to `.git/modules/**/hooks` and also add the content of the `default.sh` file to all `pre-commit` files. 

By doing this every commit will be inspected and either pass without warnings, pass with warnings or stop with errors. This will allow you to fix code before committing it. Be aware only changed files will be inspected. Also make sure all `pre-commit` have `+x` permissions.