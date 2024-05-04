---
layout: post
title: Box-Drawing Mojibake
---

At $dayjob I recently came across the string "f├╝r" and was wondering what kind of [Mojibake] that was.

From context it was obvious that it should be "für", i.e. the Umlaut aka "Latin Small Letter U with Diaeresis"

Turns out the answer is most likely: UTF-8 passed through a [CP437] (orginal IBM-PC character set), [CP850]
(Latin-1), or [CP852] (Latin-2) to UTF-8 decoder. 'ü' is [U+00FC], which in UTF-8 is `0xC3 0xBC`.

If we look those up in CP437, we get `├` (U+251C) for 0xC3 and `╝` (U+255D) for 0xBC.

[Mojibake]: https://en.wikipedia.org/wiki/Mojibake
[CP437]: https://de.wikipedia.org/wiki/Codepage_437
[CP850]: https://de.wikipedia.org/wiki/Codepage_850
[CP852]: https://de.wikipedia.org/wiki/Codepage_852
[U+00FC]: https://www.compart.com/de/unicode/U+00FC
