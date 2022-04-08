# Drag & Drop

Elements can be made drag and droppable which can be helpful to allow users to re-order and re-position elements in a list, table or customize the user interface.

## Container

In order to make elements drag and droppable you must define a container which contains all the drag and droppable elements.

```html
<div class="dragcontainer">
</div>
```

## Draggable elements

Elements which should be draggable in the container must be marked with `draggable="true"`.

```html
<div class="dragcontainer">
    <div draggable="true">1</div>
    <div draggable="true">2</div>
    <div draggable="true">3</div>
    <div draggable="true">4</div>
</div>
```

> If new draggable elements are dynamically added to the container they automatically get recognized, no additional event binding is necessary.

![Button normal](Developer-Guide/frontend/elements/dragndrop/dragndrop.png)