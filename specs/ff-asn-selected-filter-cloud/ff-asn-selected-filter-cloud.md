# ff-asn-selected-filter-cloud
The `ff-asn-selected-filter-cloud` displays all active filters using `ff-asn-group-element`'s.

## Usage
```html
<ff-asn-selected-filter-cloud>
    <ff-asn-group-element>
        <span slot="selected">{{name}}</span>
        <span slot="unselected">{{name}}</span>
    </ff-asn-group-element>
</ff-asn-selected-filter-cloud>

<!-- OR -->

<ff-asn-selected-filter-cloud>
    <section>
        <!-- Will be rendered inside this section -->
        <ff-asn-group-element>
            <span slot="selected">{{name}}</span>
            <span slot="unselected">{{name}}</span>
        </ff-asn-group-element>
    </section>
</ff-asn-selected-filter-cloud>
```

### Order of filter elements
You can control the order of the filter elements by using the `order` attribute. The `order` attribute allows the following 3 values:

**fact-finder-group-order** (Default): Sorts the filter elements in the order provided by FACT-Finder. (Can be configured in the FF UI). <br>
**alphabetically**: Sorts the filter elements alphabetically. <br>
**user-selection**: Sorts the filter elements in the order the user has clicked them.

#### example order usage
```html
<ff-asn-selected-filter-cloud order="user-selection">
    <ff-asn-group-element>
        <span slot="selected">{{name}}</span>
        <span slot="unselected">{{name}}</span>
    </ff-asn-group-element>
</ff-asn-selected-filter-cloud>
```

### blacklist
If you want to exclude specific filter groups from the filter cloud you can use the `blacklist` attribute.

**Note**
This attribute can not be used in conjunction with the **whitelist** attribute.

#### example blacklist usage
This example will blacklist all filters applied by the `Price` and `Size` groups.
```html
<ff-asn-selected-filter-cloud blacklist="Price,Size">
    <ff-asn-group-element>
        <span slot="selected">{{name}}</span>
        <span slot="unselected">{{name}}</span>
    </ff-asn-group-element>
</ff-asn-selected-filter-cloud>
```

### whitelist
If you want to exclusively specify each filter group you can use the `whitelist` attribute.

**Note**
This attribute can not be used in conjunction with the **blacklist** attribute.

#### example whitelist usage
This example will whitelist only filters applied by the `Gender` and `Catgeory` groups.
```html
<ff-asn-selected-filter-cloud whitelist="Gender,Category">
    <ff-asn-group-element>
        <span slot="selected">{{name}}</span>
        <span slot="unselected">{{name}}</span>
    </ff-asn-group-element>
</ff-asn-selected-filter-cloud>
```

## API
### Properties
| Name | Description |
| ---- | ----------- |
| **order**&nbsp;(String) **Options**:&nbsp;"alphabetically,user-selection,fact-finder-group-order",&nbsp; (default: "fact-finder-group-order") | **alphabetically**: Sorts the filter elements alphabetically. <br>**fact-finder-group-order**: Sorts the filter elements in the order provided by FACT-Finder. (Can be configured in the FF UI). <br> **user-selection**: Sorts the filter element in the order the user has clicked them. |
| **blacklist**&nbsp;(String) | A comma separated list of asn group names which should **not** appear in the list of selected filters. _This attribute can not be used in conjunction with the **whitelist** attribute._ |
| **whitelist**&nbsp;(String) | A comma separated list of asn group names which should **exclusively** appear in the list of selected filters. _This attribute can not be used in conjunction with the **blacklist** attribute_ |

### Events
| Name | Description |
| ---- | ----------- |
| **dom-updated** |  This event is triggered when the element has received new data and the template for this element and all sub elements have been punched out. |
