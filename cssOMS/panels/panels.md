# Panels

Panels are for displaying content (e.g. forms, text) in a uniform style across the application.

## Example

### Plain

![Panel normal](Developer-Guide/frontend/elements/panels/plain.png)

```html
<div class="row">
    <div class="col-xs-6">
        <section class="box wf-100">
            <div class="inner">....</div>
        </section>
    </div>
</div>
```

### Header & footer

![Panel header & footer](Developer-Guide/frontend/elements/panels/header_footer.png)

```html
<div class="row">
    <div class="col-xs-6">
        <section class="portlet">
            <div class="portlet-head">Header</div>
            <div class="portlet-body">...</div>
            <div class="portlet-foot">Footer</div>
        </section>
    </div>
</div>
```