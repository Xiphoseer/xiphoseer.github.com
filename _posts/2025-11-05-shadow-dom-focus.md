---
title: Shadow DOM, isConnected, and focus
author: Xiphoseer
---

I've been working with https://lit.dev elements and want to summarize
what I've learned about Shadow DOM and Focus:

- When a [custom element] is created with `document.createElement('my-el')`
  it's constructor is run. It has an `ownerDocument`, but [`isConnected`] is `false`
  and `shadowRoot` is null.
- When the element is `append`ed to a DOM element it becomes _connected_ to
  the document. [`isConnected`] will be true and the `connectedCallback` is
  enqueued by the browser, even if it may not run immediately. A call to
  `getRootNode({composed: true})` will return the document.
- When `attachShadow` is called, the `shadowRoot` will be initialized and take
  over the elements place in the render tree. That means that past this point,
  any element in the light dom that's not assigned to a slot will not have
  any `offsetParent`, which means that the element has no render box and
  is not considered _being rendered_.
- The element being rendered is one of the requirements for being a
  [_focusable area_][focusable-area]:
  1. the element's _tabindex_ value is non-null, or the element is determined by the user agent to be focusable;
  2. the element is either not a _shadow host_, or has a _shadow root_ whose _delegates focus_ is `false`;
  3. the element is not _actually disabled_;
  4. the element is not _inert_;
  5. the element is either _being rendered_, _delegating its rendering to its children_, or _being used as relevant canvas fallback content_.

Thus, if you want to call [`focus`] on an element in _light dom_ of a custom
element and have it actually move the focus and dispatch the `focus` event,
ensure all the shadow dom slots are initialized.

[`focus`]: https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/focus
[`isConnected`]: https://developer.mozilla.org/en-US/docs/Web/API/Node/isConnected
[custom element]: https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_custom_elements
[focusable-area]: https://html.spec.whatwg.org/multipage/interaction.html#focusable-area
