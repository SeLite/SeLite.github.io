---
layout: default
---
{% include links %}

= Overview =
Preview visually presents custom HTML reports or forms. It allows Selenese scripts to present data. It can also collect confirmation, selection or data from the user. Selenese scripts can run further processing depending on the user's decision.

The presentation is through HTML, possibly with client-side template, such as [PURE](https://github.com/pure/pure). Together with [SelBlocksGlobal](SelBlocksGlobal) > [Asynchronous Selenese calls](SelBlocksGlobal#asynchronous-selenese-calls) it enables a bidirectional channel between [Selenium IDE](SeleniumIDE) and the presentation window. The reports or forms can subsequently trigger further Selenese actions by calling Selenese functions. That allows user scripts to run interactively.

Presentation layer renders in a Firefox "chrome" window. That looks like a part of Firefox GUI, rather than a webpage. Hence the user can differentiate it from the webpage intuitively. That also saves screen space.

= Limitations =
The custom HTML must define basic formatting in CSS, since "chrome" layer doesn't include default HTML style. Links can't point to URLs. Instead, use `onclick="..."` to invoke Selenese functions through `selenium.callFromAsync(...)`.

Previews windows are not bookmarkable and the user can't navigate backwards & forwards. Otherwise links in a preview could be in conflict with Selenese state (call stack, stored variables) once the user executes any further Selenese commands.
<!-- TODO try open in FF sidebar. Have it configurable via Settings. -->
