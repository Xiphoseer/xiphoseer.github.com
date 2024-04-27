---
title: Expression Languages
date: 2024-04-26
---

I recently realized that one common aspect of my interests in computer science
so far has been *expression languages* (EL). For the purpose of this article,
I'll define it as follows:

> An expression language is syntax for representating pure, computable functions

Importantly, there can be multiple ways to represent the same logical expression.

One very well known example of an expression language is mathematical
notation, or rather the subset that can be used in a calculator.

All of the supported operations are pure (i.e. free from sideeffects) and
are computable (i.e. have a well-defined way to compute the result, be
it using normal mathematical operations or methods from numerical analysis).

Another example are the expressions used in functional programming languages.
These are built on the same theory as mathematical notation, so it's no
surprise that they are equivalent in this regard.

## Graphical Expression Languages

What makes expression *languages* interesting to me is that they are
a finite encoding of the pure function. It's possible to extend this
to _any_ pure function in theory simply by introducing a symbol for
that function.

This makes it possible to represent the rules of computation of that
function visually. For example, `sin(x)` might be the textual representation
of the sine function in a programming language.

## BPMN 2.0 EL

The OMG Standard Business Process Modeling and Notation (BPMN 2.0) uses
an expression language to define the data I/O between activities.

## GreenPass Rules

During the Covid-19 Pandemic, a simple, JSON based expression language
was used to check a digital vaccine certificate for validity in a particular
country according to a set of validation rules in that EL.

## Program Visualization