# Layout

The following layout allows you to define and structure content based on screen/browser window resoltion. Internally this implementation uses `flexbox`.

## Examples

![Grid](Developer-Guide/frontend/grid/grid.png)

Available sizes are `xs-*`, `sm-*`, `md-*`, `lg-*` with a grid ranging from `1-12`. Every prefix (e.g. `xs-*`) represents a window size when it becomes active. It's also possible to combine them to define a layout for different browser window sizes. This is useful for different devices such as desktop, cellphone, tablet.

* `xs-` extra small
* `sm-` small
* `md-` medium
* `lg-` large


```html
<div class="row">
    <div class="col-xs-12">
        <section class="box wf-100">&nbsp;</section>
    </div>
</div>

<div class="row">
    <div class="col-xs-6">
        <section class="box wf-100">&nbsp;</section>
    </div>
    <div class="col-xs-6">
        <section class="box wf-100">&nbsp;</section>
    </div>
</div>

<div class="row">
    <div class="col-xs-4">
        <section class="box wf-100">&nbsp;</section>
    </div>
    <div class="col-xs-4">
        <section class="box wf-100">&nbsp;</section>
    </div>
    <div class="col-xs-4">
        <section class="box wf-100">&nbsp;</section>
    </div>
</div>

<div class="row">
    <div class="col-xs-3">
        <section class="box wf-100">&nbsp;</section>
    </div>
    <div class="col-xs-3">
        <section class="box wf-100">&nbsp;</section>
    </div>
    <div class="col-xs-3">
        <section class="box wf-100">&nbsp;</section>
    </div>
    <div class="col-xs-3">
        <section class="box wf-100">&nbsp;</section>
    </div>
</div>

<div class="row">
    <div class="col-xs-9">
        <section class="box wf-100">&nbsp;</section>
    </div>
    <div class="col-xs-3">
        <section class="box wf-100">&nbsp;</section>
    </div>
</div>

<div class="row">
    <div class="col-xs-8">
        <section class="box wf-100">&nbsp;</section>
    </div>
    <div class="col-xs-4">
        <section class="box wf-100">&nbsp;</section>
    </div>
</div>

<div class="row">
    <div class="col-xs-6">
        <section class="box wf-100">&nbsp;</section>
    </div>
    <div class="col-xs-3">
        <section class="box wf-100">&nbsp;</section>
    </div>
    <div class="col-xs-3">
        <section class="box wf-100">&nbsp;</section>
    </div>
</div>

```