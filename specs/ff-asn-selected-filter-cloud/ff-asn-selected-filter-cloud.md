# ff-filter-cloud
The `ff-filter-cloud` displays all filters that are currently selected through `ff-asn`.

## Usage

Minimum setup:

```html
<ff-filter-cloud>
      <span data-template="filter">{{group.name}}: {{element.name}}</span>  <!-- this element is repeated -->
</ff-filter-cloud>
```

With additional custom HTML:

```html
<ff-filter-cloud>
    <h4>The Filter Cloud</h4>

    <div>
        <div>Custom above</div>
        
        <!-- template is stamped for each selected filter at this position --> 
        <div data-template="filter" class="filter-element">
            <span>{{group.name}}: {{element.name}} ×</span>
        </div>
        
        <div>Custom below</div>
  </div>
</ff-filter-cloud>
```

### Order of filter elements
You can control the order of the filter elements by using the `order` attribute. The `order` attribute allows the following 3 values:

**fact-finder** (Default): Sorts the filter elements in the order provided by FACT-Finder. (Can be configured in the FF UI).  
**alphabetical**: Sorts the filter elements alphabetically.  
**user-selection**: Sorts the filter elements in the order the user has clicked them.

#### example order usage
```html
<ff-filter-cloud order="user-selection">
    <div>
        <span data-template="filter">{{group.name}}: {{element.name}}</span>
    </div>
</ff-filter-cloud>
```

### blacklist
If you want to exclude specific filter **groups** from the filter cloud, you can use the `blacklist` attribute. You may specify as many groups as you need, separated by commas. The values must correspond to the group `name` or `associatedFieldName` property returned by FACT-Finder.

**Note**
This attribute cannot be used in conjunction with the `whitelist` attribute. Doing so will cause an error.

#### example blacklist usage
This example will blacklist all filters applied by the `Price` and `Size` groups.
```html
<ff-filter-cloud blacklist="Price,Size">
    <div>
         <span data-template="filter">{{group.name}}: {{element.name}}</span>
    </div>
</ff-filter-cloud>
```

### whitelist
If you want to exclusively specify each filter **group**, you can use the `whitelist` attribute. Like in `blacklist`, you may specify as many groups as you need, separated by commas. The values must correspond to the group `name` or `associatedFieldName` property returned by FACT-Finder.

**Note**
This attribute cannot be used in conjunction with the `blacklist` attribute. Doing so will cause an error.

#### example whitelist usage
This example will whitelist only filters applied by the `Gender` and `Catgeory` groups.
```html
<ff-filter-cloud whitelist="Gender,Catgeory">
    <div>
         <span data-template="filter">{{group.name}}: {{element.name}}</span>
    </div>
</ff-filter-cloud>
```

## Default templates

### Setup
```html
<ff-filter-cloud></ff-filter-cloud>
```

### Rendered HTML
```html
<ff-filter-cloud>
    <span data-template="filter">Filter 1</span>
    <span data-template="filter">Filter 2</span>
    <!-- ... -->
    <span data-template="filter">Filter n</span>
</ff-filter-cloud>
```
---

### Setup
```html
<ff-filter-cloud>
    <div>
        <span data-template="filter">
            <span>Custom: {{group.name}}: {{element.name}}</span>
            <span>×</span>
        </span>
    </div>
</ff-filter-cloud>
```

### Rendered HTML
```html
<ff-filter-cloud>
    <div>
        <span data-template="filter">
            <span>Custom: Group1: Filter 1</span>
            <span>×</span>
        </span>
        <span data-template="filter">
            <span>Custom: Group1: Filter 2</span>
            <span>×</span>
        </span>
        <!-- ... -->
        <span data-template="filter">
            <span>Custom: GroupN: Filter N</span>
            <span>×</span>
        </span>
    </div>
</ff-filter-cloud>
```


## Directives

| Attribute     | Options  | Required | Notes
| ---------     | -------- | -------- | -----------
| data-template | filter   | optional | (TODO) Warning in console if not specified

```html
<div data-template="filter">{{group.name}}: {{element.name}}</div>
```

## API

### Attributes

| Attribute | Required | Type   | Options        | Default     | JS property
| --------- | -------- | ----   | -------        | -------     | -----------
| order     | optional | String | _fact-finder_  | fact-finder | `order`
|           |          |        | alphabetical   |             |
|           |          |        | user-selection |             |

Changing this value causes the `ff-filter-cloud` to update. It will not issue a request to FACT-Finder.

_Setting a value other than listed in the Options column causes an error._

#### Options

| Option | Description
| ------ | -----------
| `fact-finder`    | Sorts the filter elements in the order provided by FACT-Finder. (Can be configured in the FACT-Finder UI)
| `alphabetical`   | Sorts the filter elements alphabetically regardless of their filter group
| `user-selection` | Sorts the filter elements in the order the user has clicked them

---

| Attribute | Required | Type         | Default  | JS property
| --------- | -------- | ----         | -------  | -----------
| blacklist | optional | String (CSV) | _(none)_ | `blacklist`

A comma separated list of asn group names which should **not** appear in the list of selected filters. Changing this attribute has no effect until the next search request to FACT-Finder.

Note that the comparison of group names is case-sensitive.

Changing this value causes the `ff-filter-cloud` to update. It will not issue a request to FACT-Finder.

_This attribute cannot be used in conjunction with the `whitelist` attribute. Doing so will cause an error._


---

| Attribute | Required | Type         | Default  | JS property
| --------- | -------- | ----         | -------  | -----------
| whitelist | optional | String (CSV) | _(none)_ | `whitelist`

A comma separated list of asn group names which should **exclusively** appear in the list of selected filters. Changing this attribute has no effect until the next search request to FACT-Finder.

Note that the comparison of group names is case-sensitive.

Changing this value causes the `ff-filter-cloud` to update. It will not issue a request to FACT-Finder.

_This attribute cannot be used in conjunction with the `blacklist` attribute. Doing so will cause an error._


### Events
| Name | Description |
| ---- | ----------- |
| **dom-updated** |  This event is triggered when the element has received new data and the template for this element and all sub elements have been punched out. |

## Dev Notes

There shall be only **one** delegated click listener for the filter elements. It is attached to the `ff-filter-cloud` element itself. Click events on filter items shall bubble up to `ff-filter-cloud` and be processed there.
This shall allow users to attach their own listeners closer to the filter items and `.stopPropagation()` to prevent default behaviour.

```html
<ff-filter-cloud>  <!-- our event listener attached here -->
    <div>
        <span data-template="filter" onclick="function userOnClick(e) { e.stopPropagation(); }">
            <span>Custom: {{group.name}}: {{element.name}}</span>
            <span>×</span>
        </span>
    </div>
</ff-filter-cloud>
```
