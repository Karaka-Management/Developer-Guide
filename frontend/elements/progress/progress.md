# Progress

A progress bar can be used to show the prograss of a background process, milestone progress, etc.

## Examples

### Progres bar

![Progress](Developer-Guide/frontend/elements/progress/progress.png)

```html
<progress value="33" max="100"></progress>
```

### Meter without stripes

![Meter](Developer-Guide/frontend/elements/progress/meter.png)

```html
<div class="meter green nostripes">
    <span style="width: 50%"></span>
</div>
```

#### Meter striped without animation

![Meter striped](Developer-Guide/frontend/elements/progress/meter_striped.png)

```html
<div class="meter green noanimation">
    <span style="width: 25%"></span>
</div>
```

For a meter with animations remove the `noanimation` class.