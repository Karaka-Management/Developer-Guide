# Developer Environment

The following points are only recommendations to help with the development process. Support for other settings or tools cannot be guaranteed.

## IDE/Editor

The following IDEs and editors are recommended

1. PHPStorm
2. Visual Studio Code
    1. Markdown All in One
    2. PHP Debug
    3. PHP IntelliSense
    4. PHP Server
    5. PHPUnit
    6. SVG Viewer

## Tools

The following tools are important to test the application and to ensure the code quality. The configurations and sample shell executions can be found in the `Build` directory. These tools are also downloaded during the setup process of the `buildProject.sh` script (see installation documentation).

* composer
* phploc
* phpunit
* phpcs
* phpmd
* phpmetrics
* pdepend
* documentor
* phpstan
* phan

## Git Hooks (Linux only)

For developers it is recommended to copy the contents of the `default.sh` file in the `Build` repository under `Hooks` to your `pre-commit` file in the `.git/hooks` directory. If the `pre-commit` file doesn't exist just create it.

The same should be done with every module. Simply go to `.git/modules/**/hooks` and also add the content of the `default.sh` file to all `pre-commit` files. 

By doing this every commit will be inspected and either pass without warnings, pass with warnings or stop with errors. This will allow you to fix code before committing it. Be aware only changed files will be inspected. Also make sure all `pre-commit` have `+x` permissions.