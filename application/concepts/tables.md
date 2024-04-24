# Tables/Data Lists

Tables/data lists are used across all of the application. For this reason filtering, sorting and extracting data is a integral part of the application.

## Sort Tables/Data Lists

The goal is to provide users with the option to sort data freely according to their needs. This includes sorting tables/data lists based on multiple (in many cases even all) columns.

For performance reasons most data is sorted without the `offset` sql keyword.

If a table/data list is sorted by another column than the primary field it will use the primary field as secondary descending sort option. The reason for this is that in most situations users want to see the newest elements first.

Url parameters:

* `element` = which element is getting sorted. Only one table/data list on a page can be sorted.
* `id` = base id for the pagination. This id is most of the time the primary field of the model
* `ptype` = pagination type.
  * This parameter can be `p` for previous page or `n` for next page and defines how the base id should be interpreted.
  * `p` means that elements to the **left/above** of `id` should be shown
  * `n` means that elements to the **right/below** of `id` should be shown
* `sort_by` defines the column which is used to sort the data
* `sort_order` defines what **left/above** and **right/below** mean
  * `ASC` means ascending and therefore defines **left/above** as smaller elements and **right/below** as larger elements
  * `DESC` means descending and therefore defines **left/above** as larger elements and **right/below** as smaller elements
* `subid` defines the id of the `sort_by` column IFF `sort_by` is not the primary field.

> The maximum amount of elements returned depend on the defined limit.

An example sort query may look like this:

```html
?sort_by=module&sort_order=DESC&id=123&subid=Backend&ptype=n
```

This returns a data list with:

* data sorted by the module parameter
* the module parameter is sorted descending (Z -> A)
* the module parameter should start with "Backend" or smaller
* only data with an ID smaller than 123 should be returned (**no** data with module parameter "Backend" and id 124 or larger should be returned)

## Filter Tables/Data Lists

## Limit Data

If no limit is defined tables/data lists often return up to 25 or 50 elements. User defined limits are mostly done by providing the `limit` parameter. A limit of 0 returns all results (this may be blocked for frontend output)

Example

```html
?limit=75
```

## Export Data
