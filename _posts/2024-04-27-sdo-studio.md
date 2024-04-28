---
layout: post
title: SDO Studio v0.0.1
author: Xiphoseer
---

During the pandemic I created a small tool that would read and
transform documents created using [Signum!][ASH-SIG] 1/2 for the
Atari ST. It's a CLI application, initially created for debugging
the format, so the user experience is suboptimal.

I've been planning to create a GUI for it for a while and
have prototyped that with [rustwasm][rustwasm] tools (wasm-pack)
and the `wasm-bindgen` crate / proc-macro as part of `sdo-web`.

The first usable version of that has been added to the SDO-Toolbox
CI build and is now available at <https://sdo.dseiler.eu/studio/>.

It's very much a work in progress, but it already supports the
`imgseq` generator i.e. render pages as images using original
printer (or, if unavailable, editor) font files.

To use it, first pick the editor and printer fonts (i.e. *.E24
and *.P24, *.P09, or *.L30) files and click on "Add to collection"

![SDO Studio Fonts](/res/sdo-studio-fonts.png)

Then, click the file-picker again and select the document(s)
you want to view. You'll be presented with a list of all the
documents selected, their creation and modification dates,
the number of embedded images and the character sets specified
within the file.

![SDO Studio Document List](/res/sdo-studio-doc-list.png)

Yellow means that not all kinds of printer fonts are available
in the collection (which uses the [Origin Private File System (OPFS)][OPFS]
to store files in the browser), red means the editor font is
missing.

Clicking on the document will render the content (which may
take some time for larger documents) and display the sequence
of images:

![SDO Studio Doc Example](/res/sdo-studio-imgseq.png)

If you're interested in the development of this tool or the
underlying features of the toolbox (such as PDF generation)
or want to provide feedback or suggestions, you can find the
official docs of the project at <https://sdo.dseiler.eu> and
the GitHub Repository at <https://github.com/Xiphoseer/sdo-tool>.

[ASH-SIG]: https://application-systems.de/signum
[rustwasm]: https://rustwasm.github.io/ 
[OPFS]: https://developer.mozilla.org/en-US/docs/Web/API/File_System_API/Origin_private_file_system