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

[quick-xml]: https://crates.io/crates/quick-xml
[XPath]: https://www.w3.org/TR/xpath
