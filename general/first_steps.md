# First steps

After you [installed]({%}?page=general/setup.md) the application and configured your development environment you are ready to make your first code contributions.

Please note that besides the general development guide the organization also provides various other organizational documents which help to understand the processes, development status and decisions made.

* [Development process](https://github.com/Karaka-Management/Organization-Guide/blob/master/Processes/01_Development.md)
* [Code inspection]({%}?page=quality/inspections)
* [Project status](https://github.com/orgs/Karaka-Management/projects/10)
* [Code of conduct](https://github.com/Karaka-Management/Organization-Guide/blob/master/Policies%20%26%20Guidelines/Code%20of%20Conduct.md)
* [Conflict of interest](https://github.com/Karaka-Management/Organization-Guide/blob/master/Policies%20%26%20Guidelines/Conflict%20of%20Interest%20Policy.md)
* [Activity Policy](https://github.com/Karaka-Management/Organization-Guide/blob/master/Policies%20%26%20Guidelines/Organization%20Activity%20Policy.md)
* [Organization Guidelines](https://github.com/Karaka-Management/Organization-Guide/blob/master/Policies%20%26%20Guidelines/Organization%20Guidelines.md)

## First tasks

### Unit tests & code coverage

Implement tests to improve code coverage. Uncovered lines can be found in the coverage [overview](https://dev.jingga.app/src/Karaka/build/coverage/).

### Documentation

#### Test documentation

All tests need to have the following docblocks:

##### Class

/**
 * @testdox phpOMS\tests\Image\SkewTest: Image skew
 * @internal
 */

* @testdox Is a one-line test description which is included in a test report for customers. The **FQN is very important**, it must be present.

##### Function

```php
/**
 * @testdox A image can be automatically unskewed
 * @group framework
 * @covers phpOMS\Image\Skew
 */
```

* @testdox Is a one-line test description which is included in a test report for customers.
* @group Is mostly `framework` (for phpOMS) or `module` for (for Modules)
* @covers Is used to restrict the class which is getting covered by this test

#### Module documentation

Modules have a `Help` and a `Dev` documentation both are insifficient for most modules. Feel free to add some documentation. Consider to use images wherever helpful. Consider to add the used images to https://github.com/Karaka-Management/Build/blob/master/Js/createImages.js which will automatically create new images even if the style changes or minor layout changes are made.

```js
...
    [
        'http://192.168.178.38/en/admin/module/settings?id=Admin#c-tab-3',
        '//*[@id="content"]',
        __dirname + '/../../Modules/Admin/Docs/Help/img/admin-module-admin-settings-design.png'
    ],
...
```

1. Url to the endpoint (must use the same IP used in other examples)
2. XPath of the content you want to take an image from
3. Output directory

### Todos

Usually todos with **low** priority and **easy** difficulty are good beginner todos: https://github.com/orgs/Karaka-Management/projects/10.
