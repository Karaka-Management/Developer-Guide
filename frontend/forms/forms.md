# Forms

Handling data is very important for the application which makes forms and data display very important.

Basically any element that can contain child elements can be a form, this includes `<form>` elements but also elements which are marked as form with a data binding `data-tag="form"`. This gives a lot of options as now divs or tables can be used as forms.

**Important:** Every form MUST have an ID defined, if this attribute is not set an element will not get recognized as form, even if it is the `<form>` element itself!

## Bindings

data-tag : tells the element how to behave (e.g. turn none-form elements into forms by defining it as "form")

data-method : method definition for none-form elements (not used for actual forms, where method can be used)
data-method-delete : delete request endpoint if a special endpoint is required

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
- [ ] Remote Remove (easy, only submit data)
- [ ] Remote Update on change (no save button required)

### External
- [x] add
- [x] update
- [x] cancel
- [ ] remote add (easy, only submit data)
- [ ] remote update (easy, only submit data)

### Internal
- [ ] add
- [ ] update
- [ ] cancel
- [ ] remote add (easy, only submit data)
- [ ] remote update (easy only submit data)

### Heartbeat (not form specific)
- [x] Remote adds (heartbeat checks)
- [x] Remote updates (heartbeat checks)
- [x] Remote removes (heartbeat checks)
