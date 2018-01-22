# Documentation

The ixo project is made up of five key repositories namely:

- [schema](#schema) 
- [ixo-node](#ixo-node)
- [ixo-module](#ixo-module) 
- [ixo-web](#ixo-web) 
- [mkdocs](#mkdocs) [github](https://github.com/ixofoundation/mkdocs)

##Schema 
[github](https://github.com/ixofoundation/schema)

A repository for holding schema templates for the ixo Protocol

### Folder Structure
`/projects` - Project Templates

`/agents` - Agent Templates

`/claims` - Claim Templates

`/evaluations` - Evaluation Templates

### Template Schemas
All Templates follow the [JSON-LD](https://developers.google.com/search/docs/guides/intro-structured-data) structure examples can be found at [Schema.org](http://schema.org)

Example:
```
{
  "@context": "http://schema.org",
  "@type": "Organization",
  "url": "http://www.example.com",
  "name": "Unlimited Ball Bearings Corp.",
  "contactPoint": {
    "@type": "ContactPoint",
    "telephone": "+1-401-555-1212",
    "contactType": "Customer service"
  }
}
```

### Templates
Templates describe the labels, input type and the optionality of fields.  These are used on the client side for rendering schemas.

Templates consist of a single data element called `entities` which is an array of the fields/entities that need to be rendered on screen. Each entity has three mandatory values:

`label` - The label for the input

`name` - The name of the input which must be present in the schema and uses dot notation for complex types

`type` - The which describes how the input should be captured

`optional` - Determines whether the entity is required or optional (Defaults to `false`)

`options` - Only used if the type is `select` and contains a list of `labels` and `values`.

Example:
```
"options": [
  {
    "label": "Identity Document", 
    "value": "id"
  },
  {
    "label": "Passport",
    "value": "passport"
  }
]
```

#### Template Types
`text` - Renders a text input

`textarea` - Renders a multiline text input

`country` - Renders a selection box containing a list of country names and returns the (ISO 3166-2)[https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2]

`select` - Generic selection box containing a list of options.  If this type is selected the `option` value must also be set.

`image` - Renders an image or allows one to be uploaded


##ixo-Node
[github](https://github.com/ixofoundation/ixo-node)

The backend of the ixo protocol which defines the nodes that run the network
*TODO*

##ixo-Module
[github](https://github.com/ixofoundation/ixo-module)

An npm package that wrappers the communication to the ixo-node and provides usable services to a Javascript application.
*TODO*

##ixo-Web
[github](https://github.com/ixofoundation/ixo-web)

A reference implemenation of a web frontend for the ixo protocol
*TODO*

##mkdocs
[github](https://github.com/ixofoundation/mkdocs)

The repository contains the documentaion for the ixo system and is build using mkdocs.  It builds the content for this site.



