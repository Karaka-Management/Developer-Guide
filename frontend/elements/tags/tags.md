# Tags

Tags or badges are often used to describe a category or type of an element. Alternatively they are also used to highlight dynamically added objects such as objects selected from a select/datalist/input list.

## Examples

### Plain

![Tag normal](Developer-Guide/frontend/elements/tags/normal.png)

```html
<span class="tag">Test Tag</span>
```

#### Color

Since tags are often styled individually (e.g. per category, by user) we recommend to use inline style sheet for defining different background colors.

![Tag colored](Developer-Guide/frontend/elements/tags/dynamic_style.png)

```html
<span class="tag" style="background: #be2edd">Test Tag</span>
```

### Icon

![Tag icon](Developer-Guide/frontend/elements/tags/icon.png)

```html
<span class="tag"><i class="fa fa-lightbulb-o"></i>Test Tag</span>
```

### Remove and icon

![Tag remove icon](Developer-Guide/frontend/elements/tags/remove_icon.png)

```html
<span class="tag"><i class="fa fa-times"></i><i class="fa fa-lightbulb-o"></i>Test Tag</span>
```