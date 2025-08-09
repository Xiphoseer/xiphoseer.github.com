---
title: Comparison of Data Types defined in XML, WASM Component Model, and Rust.
author: Xiphoseer
---

In this post I want to do a comparison of the data models in XML, WASM and Rust.
In comparison to JSON, these generally are more expressive, i.e. have more than
just one "number" type.

I'll structure this post based around groups of types: In XML that distinction
is called simple vs complex types, in Rust there is a distinction between `Copy`
types and non-`Copy` types.

### Bounded Atomic Types

[XML-Schema Part 2: Datatypes] defines an *atomic* datatype as:

> Atomic datatypes are those whose *value spaces* contain only *atomic values*.

where *atomic value* is defined as

>  An atomic value is an elementary value, not constructed from simpler values by any user-accessible means defined by this specification.

I want to further subdivide that set into datatypes that are "bounded" i.e. which have
a value spaces that is a finite set of values, and "unbounded", which are defined by
construction and can in theory have an infinite value space.

In XML, the bounded built-in datatypes are:

| XML Schema | Rust | WIT |
|---|---|---|
| `xs:boolean` | `bool` | `bool` |
| `xs:long` | `i64` | `s64` |
| `xs:int` | `i32` | `s32` |
| `xs:short` | `i16` | `s16` |
| `xs:byte` | `i8` | `s8` |
| `xs:unsignedLong` | `u64` | `u64` |
| `xs:unsigendInt` | `u32` | `u32` |
| `xs:unsignedShort` | `u16` | `u16` |
| `xs:unsignedByte` | `u8` | `u8` |
| `xs:double` | `f64` | `f64` |
| `xs:float` | `f32` | `f32` |

XML Schema Part 2 defines additional datatypes for data stored in terms 
of the gregorian calendar. These don't really have an equivalent in Rust.
The `chrono` crate can be used to store and manipulate dates.

| Name | XML Schema | Rust |
|---|---|
| Day of Month | `xs:gDay` |
| Month of Year | `xs:gMonth` | `chrono::Month` |
| Month, Day | `xs:gMonthDay` |
| Year | `xs:gYear` |
| Year, Month | `xs:gYearMonth` |

For all practical purposes, I also consider the values on timescales as bounded,
because values beyond the year 9999 while theoretically possible are not used
in practice:

| XML Schema |
|---|
| `xs:date` |
| `xs:dateTime` |
| `xs:dateTimeStamp` |
| `xs:duration` |
| `xs:dayTimeDuration` |
| `xs:yearMonthDuration` |
| `xs:time` |

This matches the requirements for a minimally conforming implementation
in [§5.4 of XML Schema](https://www.w3.org/TR/2012/REC-xmlschema11-2-20120405/datatypes.html#partial-implementation),
which further provides a bound for `xs:decimal`, which in turn makes the following bounded:

| XML Schema |
|---|
| `xs:decimal` |
| `xs:integer` |
| `xs:nonNegativeInteger` |
| `xs:positiveInteger` |
| `xs:nonPositiveInteger` |
| `xs:negativeInteger` |

But since the bounds are implementation defined, these don't have a bounded ABI or
matching definition in Rust or WIT.

### Unbounded Atomic Types

When it comes to (theoretically) unbounded types, the most important is a string type.
Both XML Schema and WIT define a string as a sequence of characters, rust additionally
enforces a UTF-8 encoding.

XML Schema additionally defines a URI type, where the main difference is a fixed
*facet* of collapsing any sequence of whitespace into single space character.

| XML Schema | Rust | WIT |
|---|---|---|
| `xs:string` | `String` / `&str` | `string` |
| `xs:anyURI` | ? | ? |

TODO: [OMG-IDL]

[XML-Schema Part 2: Datatypes]: https://www.w3.org/TR/xmlschema11-2/
[OMG-IDL]: https://www.omg.org/spec/IDL/4.2/PDF