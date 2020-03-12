# Searchable ASN Filters (ff-asn-group)

Some data sets have huge amounts of filters per filter group (150+). Too many to display all at once and too many to browse by eye.

To improve usability the `ff-asn-group` gets an _optional_ search field. It is scoped to its associated filter group.


## General

This feature is an addition to the existing `ff-asn-group`. HTML examples are limited to new parts and minimal context for focus.

The new search field is always the first element inside the collapsible `.ffw-wrapper` container. This means the filter group must be expanded in order to use it.

Users may specify a custom search field template. It is defined via the `slot="filterSearch"` attribute. As it is a slot, its location inside the `ff-asn-group` template is irrelevant.

## Features

### search as you type

Filter items are searched for while user is typing in search field. While search term is present, filter group is kind of in _"search mode"_ compared to _"regular mode"_.

- matching with `new RegExp(searchTerm, "gi")` or better algorithm
- matched substrings are wrapped in `<span class="ffw-query">`
- while search term is present, _"show more"_ disappears, matches of detailed and hidden links are merged into one list
- _match-head_ results are listed first, then all other matches
  - e.g.: search term: `bc`, result: `[bcd, bce, abc, cbc]`
  - order of items within the _match-head_ and _match-any_ groups remains unchanged from order in FF response
- not matching items are hidden
- no special treatment for already selected filters
  - selected filters are filtered just like all others

If a filter item (selected or unselected) is clicked while in _"search mode"_, the search field is cleared and the component returns to _"regular mode"_.

Any form of clearing the search field sets the component back to _"regular mode"_. This could, for example, be pressing the "X" on a `<input type="search">`.


## Default HTML

The search field is only rendered if the attribute `searchable-from` is set with a valid number on `ff-asn`.

If `searchable-from` is not set or has an invalid value, the search field will not be rendered.

### Setup
```html
<ff-asn searchable-from="31">
</ff-asn>
```

### Rendered

Assuming there are `31` or more links (_detailed_ and _hidden_ combined) in this group.

```html
<ff-asn searchable-from="31">
    <ff-asn-group>
        <div class="ffw-asn-group-container">
            <div class="ffw-wrapper">

                <div slot="filterSearch"><input></div>  <!-- this is the new part -->

                <div data-container="detailedLinks">
                ...
```

_Note that the search field is always the first item inside `.ffw-wrapper`._


## HTML of match highlighting

Matched substrings are wrapped in a `<span class="ffw-query">` element like it is done in `ff-suggest` and `ff-onfocus-suggest`.

The results of a search are rendered in a container separate from `data-container="detailedLinks"` and `data-container="hiddenLinks"`.

Assuming the search term is `ro`:

### Rendered

```html
<ff-asn-group>
    <div class="ffw-wrapper">

        <div slot="filterSearch">
            <input>
        </div>

        <div class="ffw-asn-group-searchable-results">  <!-- new class -->
            <ff-asn-group-element>  <!-- only innermost elements are shown here for brevity -->
                <span><span class="ffw-query">Ro</span>hner</span>
            </ff-asn-group-element>
            <ff-asn-group-element>
                <span>C<span class="ffw-query">ro</span>cs</span>
            </ff-asn-group-element>
            <ff-asn-group-element>
                <span>Gi<span class="ffw-query">ro</span></span>
            </ff-asn-group-element>
        </div>
        ...
```


## User specified templates

User templates are specified through the `slot="filterSearch"` attribute.

The `input` element is mandatory. An exception is thrown if it is not provided.

### Setup

```html
<ff-asn searchable-from="31">
    <ff-asn-group for-group="Manufacturer">
        <div slot="filterSearch">
            <i class="magnifying-glass"></i>
            <input type="search" placeholder="Search {{group.name}}...">
        </div>
    </ff-asn-group>
</ff-asn>
```

### Rendered

```html
<ff-asn searchable-from="31">
    <ff-asn-group for-group="Manufacturer">
        <div class="ffw-asn-group-container">
            <div class="ffw-wrapper">

                <div slot="filterSearch">
                    <i class="magnifying-glass"></i>
                    <input type="search" placeholder="Search Manufacturer...">
                </div>
```

---

Using the `not-searchable` attribute to prevent groups from becoming searchable.

Assuming there are `31` or more links (_detailed_ and _hidden_ combined) in each group.

### Setup

```html
<ff-asn searchable-from="31">
    <ff-asn-group for-group="Manufacturer">
        <div slot="filterSearch" class="customGroup1"><input></div>
    </ff-asn-group>
    <ff-asn-group for-group="Usage" not-searchable>
        <div slot="filterSearch" class="customGroup2"><input></div>
    </ff-asn-group>
</ff-asn>
```

### Rendered

```html
<ff-asn searchable-from="31">
    <ff-asn-group for-group="Manufacturer">
        <div class="ffw-asn-group-container">
            <div class="ffw-wrapper">

                <div slot="filterSearch" class="customGroup1"><input></div>
                ...
            </div>
        </div>
    </ff-asn-group>
    <ff-asn-group for-group="Usage" not-searchable>
        <div class="ffw-asn-group-container">
            <div class="ffw-wrapper">

                <!-- no [slot="filterSearch"] container due to [not-searchable] attribute -->
                ...
            </div>
        </div>
    </ff-asn-group>
```


## API

### Attributes

Attribute       | Type   | Required | Default    | JS property
--------------- | ------ | -------- | ---------- | ----------------
searchable-from | Number | no       | `Infinity` | `searchableFrom`

This attribute is to be placed on `ff-asn`. Not setting it results in the search field not being rendered.

The value refers to the **sum of _detailed_ and _hidden_ links**. An invalid value is treated as the attribute not being set and a **warning is logged**.

This attribute is **not** observed. Setting it later through JS does not trigger an update of the component. However, the new value will be taken into account after the component updates through other means.

---

Attribute      | Type    | Required | Default  | JS property
-------------- | ------- | -------- | -------- | ---------------
not-searchable | Boolean | no       | `false`  | `notSearchable`

This attribute may be set on `ff-asn-group` elements. It only has an effect if `searchable-from` is set.

By default, all `ff-asn-group` elements that satisfy the `searchable-from` condition are searchable. With this attribute a group can be prevented from being searchable. Thus, the search field will not appear.


## Considerations

- configurability of regex that is used for matching filters
  - could even be a predicate function if provided through JS
- configurability of search term being cleared after click
- search results could also be rendered in `data-container="detailedLinks"` although this would be inconsistent with the _detailedLinks_ emitted by FACT-Finder
  - a different styling for search results might also be of interest which supports the separate container approach
- providing additional properties `matches` and `totalElements` in group data to be used in mustache templates for dynamic displaying of search success
