---
title: QName-to-URI mapping
author: Xiphoseer
---

Looking deeper into the origins of representing
[namespaced elements in braced syntax]({% post_url 2024-05-05-xpath-json-encoding %}),
it looks like the issue of a canonical mapping from QName to a URI has a long history at the W3C:

The RDF Specification contains the following language:

> Within RDF/XML, XML QNames are transformed into RDF URI references by appending the XML local name to the namespace name (URI reference). For example, if the XML namespace prefix foo has namespace name (URI reference) http://example.org/somewhere/ then the QName foo:bar would correspond to the RDF URI reference http://example.org/somewhere/bar. Note that this restricts which RDF URI references can be made and the same URI can be given in multiple ways.
>
> -- [RDF/XML Syntax Specification (Revised) §5.1 Identifiers](https://www.w3.org/TR/REC-rdf-syntax/#section-Identifiers)

This method of concatenation introduces flexibility:

- `abstract` is a term in the DublinCore `http://purl.org/dc/terms/` namespace. Its canonical
  URI is `http://purl.org/dc/terms/abstract`, which the expanded form of `dcterms:abstract`
  using the concatenation method.
- `type` is a property name in the RDF `http://www.w3.org/1999/02/22-rdf-syntax-ns#` namespace.
  Its canonical URI uses an URI fragment to reference an XML ID. Here again, the expanded
  form of `rdf:type` matches the canoncial URI.

However:

- `int` is a datatype defined by the XML-Schema `http://www.w3.org/2001/XMLSchema` namespace.
  Its canonical URI is `http://www.w3.org/2001/XMLSchema#int`. In this case, applying the
  RDF QName-to-URI transformation would result in `http://www.w3.org/2001/XMLSchemaint`,
  which does _not_ match the canonical URI for that type.

Interestingly, the difference comes down to the fragment character being part of the RDF
namespace, but not so for the XML Schema namespace, possibly in order to support the
Dublin Core case.

This gave rise to a proposal to W3C [in 2001][www-rdf-comments/2001JanMar/0082], which was turned into [RDF Tracking issue rdfms-qname-uri-mapping](https://www.w3.org/2000/03/rdf-tracking/#rdfms-qname-uri-mapping).

The "resolution" for that issue was to _reject_ the enclosed proposal to make a case-distinction
in the RDF mapping algorithm based on the last character of the namespace URI.

### When is a QName Mapping valid?

There are at least two papers by W3C TAP on the matter:

- [Using Qualified Names (QNames) as Identifiers in XML Content](https://www.w3.org/2001/tag/doc/qnameids)
- [Associating Resources with Namespaces](https://www.w3.org/2001/tag/doc/nsDocuments/)

The former claims that

> The QName "x:p" is a concise, unambiguous name for the {URI, local-name} pair {"http://example.com/ns/foo", "p"}.

The important takeaway is that QNames are _contextual_, it's just a pair of Namespace URI and local part.
It does not have the same uniqueness guarantee that would follow from a URI with or without fragment identifier
because that can, by definition, only ever refer to a single resource.

An RDF ontology will always talk about terms, so it's reasonable to assume they have an identity that _can_ be mapped
to a full URI, but this isn't the case for XML Schema, where the same local part may
refer to an element, attribute, or datatype.

The braced mapping seems to originate either from [XSV] which is mentioned in [this mail](https://lists.w3.org/Archives/Public/www-rdf-interest/2001May/0055.html) or [this memo](http://jclark.com/xml/xmlns.htm) by James Clark.

For a list of related e-Mail threads, see also [this summary](https://lists.w3.org/Archives/Public/www-rdf-comments/2001JulSep/0124.html).

[www-rdf-comments/2001JanMar/0082]: https://lists.w3.org/Archives/Public/www-rdf-comments/2001JanMar/0082.html
[XSV]: https://www.cogsci.ed.ac.uk/~ht/xsv-status.html