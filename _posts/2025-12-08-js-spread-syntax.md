---
title: JS Spread syntax for arrays vs objects
author: Xiphoseer
---

Noticed a small quirk today in the JavaScript [spread syntax]: If you
have three values:

```js
const a = null;
const b = { x: 1 };
const c = [2];
```

Then the spread syntax for objects will work just fine

```js
expect({ ...a, ...b }).toStrictEqual({ x: 1 });
```

While the spread syntax for arrays will throw a _TypeError: a is not iterable_:

```js
expect([...a, ...c]).toThrow();
```

So the handling of `null` is _different_ for these two cases. Interestingly
`typeof null` seems to return `object`, while `Object.keys(null)` throws
`TypeError: Cannot convert undefined or null to object`.

At the same time, `typeof undefined === 'undefined'` but the same seems to
happen for `a = undefined` for the spread syntax (error for arrays, fine for objects).

[spread syntax]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax
