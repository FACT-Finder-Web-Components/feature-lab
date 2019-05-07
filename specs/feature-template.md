# Feature Name (e.g. element name)

Brief outline what this feature is about.


## General

Description of general and characteristics and things to be aware of during implementation and usage.

Specifications shall be mostly developer friendly with straight-to-the-point explanations. While at the same time it shall be taken into consideration that a spec text serves as the **basis for the official documentation** article.

Be as explicit as possible about what the feature **does** and what it **not does**.

The form of this template does not have to be adhered to 100%. Feel free to add or change sections and further explanation detail as you see fit.


## Default HTML

Description of an element's HTML related default behaviour. This is particularly relevant if an element allows users to specify custom templates.

#### Setup
```html
Example HTML users would put in their page source.
```

#### Rendered

```html
The expected rendered HTML.
```


## User specified templates

Detailed explanation of templates' rules and usage. In particular the effects of child elements and attributes.

ALL possible setups must be listed here.

#### Setup

```html
Example HTML users would put in their page source.
```

#### Rendered

```html
The expected rendered HTML.
```

Add as many _Setup/Rendered_ blocks as necessary.


## API

_One table per attribute/method etc._

#### Attributes

Attribute | Type | Required | Default | JS property
--------- | ---- | -------- | ------- | -----------
attr-name | String | yes    | _(none)_ | `attrName`

Description of `attr-name` and its behaviour. For example if changing it triggers a request.

#### Properties

_Properties are those that are only available through JavaScript._

Property | Type | Required | Default
-------- | ---- | -------- | -------
propName | Number | optional | 60*

Description of `propName`.

_(*) Special remarks. Default value could be inherited from FACT-Finder API._

#### Methods

##### `element.method(params)`

Description of the method's behaviour, its parameters and return value.


#### Events

Name | Description
---- | -----------
dom-updated | This event is triggered when the element has received new data and the template for this element and all sub elements have been punched out.



## Considerations

This section may contain any kind of information that is worth discussing with reviewers. This could be, for example, possible further functionality or unclear requirements.

As the section title says, whatever needs further consideration.
