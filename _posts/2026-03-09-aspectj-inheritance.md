---
title: AspectJ & Inheritance
author: Xiphoseer
---

Small non-obvious finding: When [AspectJ](https://eclipse.dev/aspectj/) is used to intercept
(public) methods of a class identified e.g. by annotation, it will only intercept the methods
that are actually implemented in that class, but not public methods on any superclass.
