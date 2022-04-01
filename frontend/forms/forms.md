# Forms

## Bindings

data-tag : tells the element how to behave (e.g. turn none-form elements into forms by defining it as "form")
data-method : method definition for none-form elements (not used for actual forms, where method can be used)

data-marker=tpl : ??? maybe entries which are templates (e.g. inline add or inline edit?) **check**
data-id : Data id (primary id value, by which it is stored in the database)

data-ui-content
data-ui-element

data-tpl-text
data-tpl-value
data-value
value

data-add-form : form template to use for the inline add **check**
data-add-content
data-add-element
data-add-tpl

data-update-form : form where updates/changes are handled (this means there is an external form where the data gets copied to for modification)
data-update-content
data-update-element
data-update-tpl

data-tpl-text-path : remote path for the text
data-tpl-value-path : remote path for the text


## Todos

### General
- [x] Order
- [ ] Remote order
- [x] DragDrop
- [ ] Remote DragDrop
- [x] Remove
- [ ] Remote Remove
- [ ] Remote Update on change (no save button required)

### Heartbeat
- [ ] Remote adds (heartbeat checks)
- [ ] Remote updates (heartbeat checks)
- [ ] Remote removes (heartbeat checks)

### External
- [x] add
- [ ] update
- [ ] remote add
- [ ] remote update
- [x] cancel

### Internal
- [ ] add
- [ ] update
- [ ] remote add
- [ ] remote update