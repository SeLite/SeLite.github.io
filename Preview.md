---
layout: default
---
{% include links %}

# Overview
Preview presents custom HTML reports or forms. It allows Selenese scripts to interact with the user. It can collect confirmation, selection or data entry. When the user operates Preview, it can trigger Selenese commands. That subsequent processing executes like classic Selenese run. The user can pause, stop or debug it.

The presentation is customizable through HTML or XML. HTML (and in XML, too) can use client-side templates, such as [PURE](https://github.com/pure/pure). Together with [SelBlocksGlobal](SelBlocksGlobal) > [Asynchronous Selenese calls](SelBlocksGlobal#asynchronous-selenese-calls) Preview enables a bidirectional channel between [Selenium IDE](SeleniumIDE) and the presentation window. The report or form can subsequently call custom Selenese functions (i.e. blocks of Selenese commands). That allows user scripts to run interactively.

# Exporting 
The content renders in a standard Firefox window. The user can bookmark it, or pass the URL to it. It uses [data: URIs](https://developer.mozilla.org/en-US/docs/Web/HTTP/data_URIs), hence it works without a need to pass any files. (data: URI can also contain images, stylesheets and Javascripts). This works for XML, too.

# Connection to Selenium IDE 
Preview is "live" and connected to Selenium IDE, but only in the window that was opened from Selenium IDE by `getEval | editor.openPreview( templateURL, data, config )`. Invoke Selenese functions through `selenium.callFromAsync(...)` from the presented content.

The user may not be able to navigate backwards & forwards in Preview. Otherwise links in Preview could be in conflict with Selenese state (call stack, stored variables) once the user executes any further Selenese commands.
<!-- TODO try open in FF sidebar.
TODO Have it configurable via Settings. -->
