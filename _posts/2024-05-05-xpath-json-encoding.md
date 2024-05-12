---
author: Xiphoseer
title: XPath Data Model for encoding XML as JSON
---

There is a model for mapping arbitrary XML to a JSON-like data model.
Consider the following document:

```xml
<document lang="de">
    <list>
        <item tag="X">A</item>
        <item tag="Y">B</item>
    </list>
    <field>Hello World</field>
</document>
```

The JSON equivalent for that looks something like:

```json
{
    "@lang": "de",
    "list": {
        "item": [
            { "@tag": "X", "$value": "A" },
            { "@tag": "Y", "$value": "B" }
        ],
    },
    "field": "Hello World"
}
```

The attribute syntax in particular (e.g. `@lang`) seems to be derived
from [XPath], where it was already in use to differentiate attributes
from elements.

One example where this model is used in the `serde` support for [quick-xml].

## Edit: 2024-05-10

If we look at the encoding for qualified names (**QName** c.f. [Namespaces in XML]) in XPath,
we can see that a **URIQualifiedName** was introduced in Version 3.0, used in the [EQName]
production. That one looks like:

```
Q{http://example.com/ns}invoice
```

This syntax could be a good candidate to encode a namespace within a
data model that only supports simple field names.

Note that the spec defines equality comparisions on expanded QNames (Namespace, Prefix, Local Part)
as only considering the namespace and the local part.

Normal QNames (e.g. `example:invoice`) are also allowed as EQNames but require the namespace to be
defined before used as a Terminal in the XPath expression.

See also: [QName-to-URI mapping]({% post_url 2024-05-11-qname-to-uri %})

[quick-xml]: https://crates.io/crates/quick-xml
[XPath]: https://www.w3.org/TR/xpath
[XPath 3.1]: https://www.w3.org/TR/xpath-31/#terminal-symbols
[EQName]: https://www.w3.org/TR/xpath-31/#doc-xpath31-EQName
[Namespaces in XML]: https://www.w3.org/TR/xml-names
