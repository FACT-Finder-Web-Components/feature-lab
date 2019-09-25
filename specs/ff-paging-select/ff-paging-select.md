# ff-paging-select

Provides paging functionality through a native `select` element.


## General

- supports custom event listeners added to `ff-paging-select` by user at any time
  ```js
  document.querySelector("ff-paging-select").addEventListener("click", handler);
  ```
  - behaviour of event listeners added to children of `ff-paging-select` is **not defined**
- uses `pageLinks` from paging response




## Default HTML

Default templates are _all-or-nothing_. They are only employed when the `ff-paging-select` element is empty. Anything else is considered a custom template and deactivates default templates.

Default `option` template corresponds to:
```html
<option>{{caption}}</option>
```

Default `select` template corresponds to:
```html
<select>
    <option>{{caption}}</option>
</select>
```

#### Setup
```html
<ff-paging-select></ff-paging-select>
```

#### Rendered

_Assuming FF is set to display 5 links._

Current page: 1
```html
<ff-paging-select>
    <select>
        <option>1</option>
        <option>2</option>
        <option>3</option>
        <option>4</option>
        <option>5</option>
    </select>
</ff-paging-select>
```

Current page: 4
```html
<ff-paging-select>
    <select>
        <option>2</option>
        <option>3</option>
        <option>4</option>
        <option>5</option>
        <option>6</option>
    </select>
</ff-paging-select>
```

## User specified templates

- user may specify the `option` element alone
- user may specify the `select` element alone (without `option`)
- user may specify the `select` element with `option`s

#### Setup

```html
<ff-paging-select>
    <option>Page {{caption}}</option>
</ff-paging-select>
```

#### Rendered

```html
<ff-paging-select>
    <select>
        <option>Page 1</option>
        <option>Page 2</option>
        <option>Page 3</option>
        <option>Page 4</option>
        <option>Page 5</option>
    </select>
</ff-paging-select>
```

#### Setup

```html
<ff-paging-select>
    <select class="user-class"></select>
</ff-paging-select>
```

#### Rendered

```html
<ff-paging-select>
    <select class="user-class">
        <option>1</option>
        <option>2</option>
        <option>3</option>
        <option>4</option>
        <option>5</option>
    </select>
</ff-paging-select>
```

#### Setup

```html
<ff-paging-select>
    <select class="user-class">
        <option>Page {{caption}}</option>
    </select>
</ff-paging-select>
```

#### Rendered

```html
<ff-paging-select>
    <select class="user-class">
        <option>Page 1</option>
        <option>Page 2</option>
        <option>Page 3</option>
        <option>Page 4</option>
        <option>Page 5</option>
    </select>
</ff-paging-select>
```

## API

### Properties

Property | Type | Required | Default | Element updates on change | DOM Attribute
-------- | ---- | -------- | ------- | ------------------------- | -------------
pagingData | Object | no   | `undefined` | yes                   | _(none)_

This property holds the data that shall be visualized by the element. It has the same structure as the FF response's `searchResult.paging` property.

**Throws** if assigned data is not in the format of `{ pageLinks: [] }`.  
Error message: `ff-paging-select received invalid data format. Must be { pageLinks: [] }`

The equivalent property on `ff-paging` is simply called `paging`.

### Events

Name | Description
---- | -----------
dom-updated | This event is triggered when the element has received new data and the template for this element and all sub elements have been punched out.


## Considerations

- `show-adjacent` attribute
  - defines how many pages before and after the current page shall be shown
  - overrides FF settings
  - max value is FF value
  ```html
  <ff-paging-select show-adjacent="2"></ff-paging-select>
  ```
- default pages in case `pageLinks` shall be overridable
    - the current page is always rendered
    - the next two pages before and after the current page are rendered whenever available
    - the first page is always _additionally_ rendered
- individual `option` templates
  ```html
  <ff-paging-select>
      <select class="user-class">
          <option type="firstLink">{{caption}}</option>
          <option type="currentLink -1">{{caption}}</option>
          <option type="currentLink">Current page {{caption}}</option>
          <option type="currentLink +1">{{caption}}</option>
      </select>
  </ff-paging-select>
  ```
