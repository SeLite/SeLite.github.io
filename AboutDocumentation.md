---
title: About SeLite documentation
layout: default
---

# Format
Having documentation in one large piece would make its structure too rigid, and maintenance and online navigation impractical. Instead, it's split in small pages. Therefore it doesn't exist in other formats. Offline viewing is easy in Firefox:
* get [Markdown Viewer](https://addons.mozilla.org/en-us/firefox/addon/markdown-viewer/) extension
* clone [documentation from GitHub](https://github.com/selite/selite.github.io) or [download it as a zip](https://github.com/selite/selite.github.io/archive/master.zip)
<!-- TODO * open local [TableOfContents](TableOfContents)-->.

We don't use GitHub Wiki, since it doesn't utilise screen well, especially on mobile devices. (E.g. its sidebar has a too wide mandatory part).

# Scope
We describe the mainstream use cases, handled in the most common ways. For more information see [PackagedTests](PackagedTests) or the source code. Then you can experiment.

SeLite doesn't cover general functionality of related technologies (see also [ReportingIssues](ReportingIssues) > [Support scope](ReportingIssues#support-scope)). If we elaborate on those topics, it's only if they are not clear or obvious from their original documentation source, or where SeLite modifies their features. Such notes and some references to third party documentation are at [SeleniumIDE](SeleniumIDE), [DevelopmentTools](DevelopmentTools), [JavascriptEssential](JavascriptEssential), [JavascriptComplex](JavascriptComplex), [InstallFromSource](InstallFromSource).

# 'scripts'
Since Selenium and SeLite are not test-specific, documentation calls test cases or suites _scripts_. They're called <i>Selenese scripts</i> only when there's a need to differentiate them from Javascript or other scripts. Some other terms are at [TestMethods](TestMethods) and [TestMethodsTheory](TestMethodsTheory).

# Firefox _chrome://_ URLs
Some of SeLite GUI is accessible via special Firefox URLs that start with _chrome://_ (which don't work in other browsers). Such links can also show source of SeLite and its Selenese references.

The documentation presents such URLs text _in italics_, but not as links. That's because Firefox doesn't allow linking to them from the internet (neither from _file://_ based URLs).

# Comments
By submitting a comment you agree that project administrator(s) can, without notice,
  * edit your comment, or
  * incorporate your comment into SeLite documentation, under the current or any future license of SeLite documentation, or
  * remove your comment.

# Details #
Implementation is mostly documented by source comments. Detailed descriptions of Selenese commands are in reference files. You can find them
  * online - see [AddOns](AddOns)
  * in Selenium IDE reference pane (once you type a chosen command in IDE).

If you read TODO subfolder, or a separate listing page [Object Diagrams](https://code.google.com/p/selite/w/list?q=label:ObjectDiagram), you may want to see [DocumentationStandard](DocumentationStandard) > [Textual object diagrams](DocumentationStandard#textual-object-diagrams).