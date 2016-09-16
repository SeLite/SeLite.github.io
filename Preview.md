---
layout: default
---
{% include links %}
* TOC
{:toc}

# Overview
Preview generates custom HTML and XML reports. It can present HTML forms. Those allow Selenese scripts to interact with the user. It can collect confirmation, selection or data entry. When the user operates Preview, it triggers further Selenese actions. That subsequent processing executes like classic Selenese run. The user can pause, stop or debug it.

The presentation in HTML or XML is customizable through client-side templates, such as [PURE](https://github.com/pure/pure). (As of March 2016, PURE is the easiest engine listed on https://developer.mozilla.org/en-US/docs/JavaScript_templates.)

Together with [SelBlocks Global] > [Out-of-flow Selenese callbacks](SelBlocksGlobal#out-of-flow-selenese-callbacks) Preview enables a bidirectional channel between [Selenium IDE](SeleniumIDEtips) and the presentation window. The report or form can subsequently call custom Selenese functions (i.e. blocks of Selenese commands). That allows user scripts to run interactively.

# Bookmarking and exporting
The content renders in a standard Firefox window. The user can bookmark it, or pass the URL to it. It uses [data: URIs](https://developer.mozilla.org/en-US/docs/Web/HTTP/data_URIs), hence it works without a need to pass any files. (`data:` URI can also contain images, stylesheets and Javascripts). This works for XML, too.

# Configuration
There are two primary ways to pass the data:

 * injecting into in-HTML Javascript. The default method. Its benefits are
   * extra 'validation' of the encoded data. (Since it's within the location part of `data:` URL, it is either well formed or not. If well formed, then both the content and the data is guaranteed to be present.)
   * easier debugging (simpler code)
   * it allows anchor (fragment) references (URLs) within the content
 * via URL hash (anchor fragment). Activated by configuration field `dataInHash`
   * benefit: you can re-use of the same content with various data by replacing the # hash (anchor fragment) part of `data:` URL
  * downsides
    * more fragile and longer `data:` URL
    * it doesn't allow anchor (fragment) references (URLs) within the content

# Debugging/readability options
By default Preview generates `data:` URL with base64 encoding. Following options make `data:` URLs more readable:

 * `urlEncodeData` - only applicable with `dataInHash`. It makes the encoded data a bit more readable (as compared to default base64 encoding).
 * `urlEncodeContent` - regardless of the source code and of how you pass the data
Those options are disabled by default, since they generate longer `data:` URLs.

# Connection back to Selenium IDE 
HTML Preview is "live" and connected to Selenium IDE, but only in the window that was opened from Selenium IDE by `getEval | editor.openPreview( templateURL, data, config )`. From the content in Preview invoke Selenese functions via [SelBlocks Global] &gt; [Out-of-flow Selenese callbacks](SelBlocksGlobal#out-of-flow-selenese-callbacks).

The user may not be able to navigate backwards & forwards in Preview. Otherwise links in Preview could be in conflict with Selenese state (call stack, stored variables) once the user executes any further Selenese commands.

We can't pass the data via query string (?param1=value2&param2=value2...) due to [data: URI limitations](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs#Common_problems).
