# Tabs

Tabs allow for further content segmentation by showing only selected content.

## Examples

### Horizontal

![Tab highlighted](Developer-Guide/frontend/elements/tabs/highlighted_horizontal.png)

```html
<div class="tabview tab-1">
    <ul class="tab-links">
        <li class="active"><label for="c-tab-1">Tab 1</label></li>
        <li><label for="c-tab-2">Tab 2</label></li>
    </ul>
    <div class="tab-content">
        <input type="radio" id="c-tab-1" name="tabular-1" checked>
        <div class="tab">
            Test content
        </div>
        <input type="radio" id="c-tab-2" name="tabular-1">
        <div class="tab">
            <p>Test content 2</p><p>asdf</p>
        </div>
    </div>
</div>
```

> In order to remove the blue outline and white background color from the content section of the tab, replace the class name `tab-1` with `tab-2`

### Vertical

![Tab highlighted](Developer-Guide/frontend/elements/tabs/highlighted_vertical.png)

```html
<div class="tabview tab-1 left">
    <ul class="tab-links">
        <li class="active"><label for="c-tab-1-2">Tab 1</label></li>
        <li><label for="c-tab-2-3">Tab 2</label></li>
    </ul>
    <div class="tab-content">
        <input type="radio" id="c-tab-1-2" name="tabular-3" checked>
        <div class="tab">
            Test content
        </div>
        <input type="radio" id="c-tab-2-3" name="tabular-3">
        <div class="tab">
            <p>Test content 2</p><p>asdf</p>
        </div>
    </div>
</div>
```

> In order to remove the blue outline and white background color from the content section of the tab, replace the class name `tab-1` with `tab-2`

### Accordion

The accordion can show and hide multiple elements. This makes it possible to also hide or show all elements at the same time.

![Accordion](Developer-Guide/frontend/elements/tabs/accordion.png)

```html
<div class="box ac-container wf-100">
    <input type="checkbox" name="About us" id="ac-1">
    <label for="ac-1">About us</label>
    <section>
        <p>...</p>
    </section>
    <input type="checkbox" name="About us" id="ac-2">
    <label for="ac-2">How we work</label>
    <section>
        <p>...</p>
    </section>
    <input type="checkbox" name="About us" id="ac-3" checked>
    <label for="ac-3">References</label>
    <section>
        <p>...</p>
    </section>
    <input type="checkbox" name="About us" id="ac-4">
    <label for="ac-4">Contact us</label>
    <section>
        <p>...</p>
    </section>
</div>
```

### More

With the more tab it's possible to show/hide content with a single click. This is perfect for created advanced content sections which should be initially hidden and only shown if the user desires to see them.

![More](Developer-Guide/frontend/elements/tabs/more.png)

![More open](Developer-Guide/frontend/elements/tabs/more_open.png)

```html
<div class="more-container">
    <input id="more-customer-region" type="checkbox">
    <label for="more-customer-region">
        <span>Click here!</span>
        <i class="fa fa-chevron-right expand"></i>
    </label>
    <div>
        <p>...</p>
    </div>
</div>
```