# ff-onfocus-suggest

The `ff-onfocus-suggest` element is kind of a two-level version of `ff-suggest`.
It suggests search terms for the text currently typed into the `ff-searchbox`.
On highlighting these suggested terms, the component displays suggestions for the highlighted search term.


## General

The first level of `ff-onfocus-suggest` works like the regular `ff-suggest` but it only displays suggestions of type `searchTerm`.
When such a search term is highlighted (via mouse hover or arrow keys), a suggest request with the highlighted term is issued.
The response is cached to avoid reissuing the same request multiple times if an item is highlighted again.
The cache is cleared once the text in the searchbox changes.


## HTML

The rendered HTML of `ff-onfocus-suggest` has a static and a dynamic part.
The static part will always be rendered independent of the custom HTML provided by the user.

#### Rendered HTML

```html
<ff-onfocus-suggest>
    <div class="ffw-suggestContainerPositioning">
        <div class="ffw-suggestContainerWrapper">
            <div class="ffw-suggestContainer"> <!-- ffw-blockLayout no longer needed -->
                <!-- section onfocus -->
                <!-- section suggestions -->
            </div>
        </div>
    </div>
</ff-onfocus-suggest>
```

The two `section` elements represent the dynamic part.


## Minimal HTML

#### Current minimum setup
```html
<ff-onfocus-suggest>
    <section data-container="onfocus"> <!-- required -->
        <ff-suggest-item type="searchTerm">{{{name}}}</ff-suggest-item> <!-- required -->
    </section>
    <section data-container="suggestions"> <!-- required -->
        <div data-container="type"> <!-- multiple elements possible, attribute required, value must match value of property 'type' in ff response -->
            <h2>Group title</h2>
            <ff-suggest-item type="type">{{{name}}}</ff-suggest-item> <!-- required, type must match data-container -->
        </div>
    </section>
</ff-onfocus-suggest>
```


#### Rendered (current)

```html
<ff-onfocus-suggest>
    <div class="ffw-suggestContainerPositioning">
        <div class="ffw-suggestContainerWrapper">
            <div class="ffw-suggestContainer ffw-blockLayout">

                <section data-container="onfocus">
                    <ff-suggest-item type="searchTerm" class="ffw-highlight-suggest-item">
                        <span class="ffw-query">sh</span></ff-suggest-item>
                    <ff-suggest-item type="searchTerm">
                        T-<span class="ffw-query">SH</span>IRT</ff-suggest-item>
                </section>

                <section data-container="suggestions">
                    <div data-container="searchTerm">
                        <h2>Group title</h2>
                        <ff-suggest-item type="searchTerm"><span class="ffw-query">sh</span></ff-suggest-item>
                        <ff-suggest-item type="searchTerm">T-<span class="ffw-query">SH</span>IRT</ff-suggest-item>
                    </div>

                    <div data-container="productName">
                        <h2>productName</h2>
                        <ff-suggest-item type="productName">Vaude - Women's Brand L/S <span class="ffw-query">Sh</span>irt - Longsleeve</ff-suggest-item>
                        <ff-suggest-item type="productName">Vaude - Jerpen L/S <span class="ffw-query">Sh</span>irt - Hemd</ff-suggest-item>
                    </div>
                </section>

            </div>
        </div>
    </div>
</ff-onfocus-suggest>
```


#### New minimum setup

```html
<ff-onfocus-suggest>
    <section data-container="onfocus"> <!-- required, is the restriction to section necessary? -->
        <ff-suggest-item>{{{name}}}</ff-suggest-item> <!-- no more type -->
    </section>
    <section data-container="suggestions"> <!-- required -->
        <div data-container="type"> <!-- one element for each type to display, data-container="type" required, value must match type in ff response -->
            <h2>Group title</h2> <!-- optional but recommended -->
            <ff-suggest-item>{{{name}}}</ff-suggest-item> <!-- required, type no longer relevant -->
        </div>
    </section>
</ff-onfocus-suggest>
```


#### Rendered (new)

```html
<ff-onfocus-suggest>
    <div class="ffw-suggestContainerPositioning">
        <div class="ffw-suggestContainerWrapper">
            <div class="ffw-suggestContainer">
                <section data-container="onfocus">

                    <ff-suggest-item class="ffw-highlight-suggest-item"> <!-- this is the currently highlighted item -->
                        <span class="ffw-query">sh</span></ff-suggest-item>
                    <ff-suggest-item>
                        T-<span class="ffw-query">SH</span>IRT</ff-suggest-item>

                </section>
                <section data-container="suggestions">
                    <!-- assuming the FF response only contained searchTerm and productName -->
                    <div data-container="searchTerm">
                        <h2>Search term</h2> <!-- assuming the user provided it like this -->
                        <ff-suggest-item type="searchTerm"><span class="ffw-query">sh</span></ff-suggest-item>
                        <ff-suggest-item type="searchTerm">T-<span class="ffw-query">SH</span>IRT</ff-suggest-item>
                    </div>
                    <div data-container="productName">
                        <h2>Product name</h2> <!-- assuming the user provided it like this -->
                        <ff-suggest-item type="productName">Vaude - Women's Brand L/S <span class="ffw-query">Sh</span>irt - Longsleeve</ff-suggest-item>
                        <ff-suggest-item type="productName">Vaude - Jerpen L/S <span class="ffw-query">Sh</span>irt - Hemd</ff-suggest-item>
                    </div>
                </section>
            </div>
        </div>
    </div>
</ff-onfocus-suggest>
```


#### Notes

Triple mustache syntax `{{{ }}}` in `ff-suggest-item` is required as the hit highlighting is realised through HTML.
With double mustache syntax the HTML would not be interpreted but displayed in its raw form.

The `section` elements are mandatory.
They must have the `data-container` attribute with the following two values.
```html
<section data-container="onfocus"></section>
<section data-container="suggestions"></section>
```

Errors if not present:
- `ERROR ff-onfocus-suggest: Required <section data-container="onfocus"> not provided.`
- `ERROR ff-onfocus-suggest: Required <section data-container="suggestions"> not provided.`

Warning* if no `ff-suggest` element is specified within `data-container="onfocus"`:
- `WARN ff-onfocus-suggest: Section onfocus must contain at least one ff-suggest-item.`

_(*) Could even be error._


## Navigating suggestions via arrow keys

When moving through the suggested items via arrow keys, the searchbox retains native focus.
Typing can be continued at any time.
The caret in the searchbox is not affected by the arrow inputs.

Pressing _Enter_ equals a mouse click on the currently highlighted item.

Movement of the highlighting is possible in all directions.
The next item to be highlighted shall be determined by the actually displayed position as much as feasible.


## API

#### Attributes

Attribute | Type   | Required | Default | JS property
--------- | ----   | -------- | ------- | -----------
subscribe | String | optional | `true`  | `subscribe`

Defines whether the component shall listen to responses issued by `ResultDispatcher`.
Updating this property makes the component subscribe or unsubscribe accordingly.

Setting it to `false` gives the user the freedom to fully control the data flow to the component.


#### Properties

Property     | Type  | Default
--------     | ----  | -------
suggestItems | Array | `undefined`

This property holds the data being displayed.
It gets updated after the component receives data.
Changing it causes `ff-onfocus-suggest` to re-render.
Can be set manually.


#### Methods

##### `element.hideSuggestionsContainer()`

Hides the element marked with `[data-container="suggestions"]` by applying inline styles.

##### `element.hideSuggestionsContainer()`

Shows the element marked with `[data-container="suggestions"]` by applying inline styles.


#### Events

Name        | Description
----        | -----------
dom-updated | This event is triggered when the element has received new data and the template for this element and all sub elements have been punched out.


## Considerations

Providing default templates for suggestion types (like `brand`, `productName`) is currently not possible.
FACT-Finder does not provide a display name to be used as a title.
Groups without a title are not useful and likely cannot even be found in the wild.
