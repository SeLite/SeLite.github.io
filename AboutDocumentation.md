---
title: About SeLite documentation
layout: default
---

# Format
Having documentation in one large piece would make its structure too rigid. Maintenance and online navigation would be impractical. Instead, it's split into small pages. Therefore it doesn't exist in other formats. Offline viewing is easy in Firefox:

* install [Markdown Viewer](https://addons.mozilla.org/en-us/firefox/addon/markdown-viewer/) extension
* clone [documentation from GitHub](https://github.com/selite/selite.github.io) or [download it as a zip](https://github.com/selite/selite.github.io/archive/master.zip).
* for navigation open [TableOfContents](TableOfContents)
* note that Markdown Viewer is not 100% proof

Alternatively, run [Jekyll locally](https://help.github.com/articles/using-jekyll-with-pages/).

SeLite doesn't use GitHub Wiki, which doesn't utilise screen well, especially on mobile devices. (E.g. its sidebar has a wide mandatory part).

# Scope
We describe the mainstream use cases, handled in the most common ways. For more information see [PackagedTests](PackagedTests) or the source code. Then you can experiment.

SeLite doesn't cover general functionality of related technologies (see also [ReportingIssues](ReportingIssues) > [Support scope](ReportingIssues#support-scope)). If it elaborates on them, it's only if they are not clear or obvious from their original documentation source, or where SeLite modifies their features. Such notes and some references to third party documentation are at [SeleniumIDE](SeleniumIDE), [DevelopmentTools](DevelopmentTools), [JavascriptEssential](JavascriptEssential), [JavascriptComplex](JavascriptComplex) and [InstallFromSource](InstallFromSource).

Implementation is mostly documented by source comments. Detailed descriptions of Selenese commands are in reference files. You can find them

  * online - see [AddOns](AddOns)
  * in Selenium IDE reference pane (once you type a chosen command in IDE).

# Terminology

## function
Word _function_ can refer to a Javascript `function` (whether in Selenium Core scope or not), or to a 'function' defined by SelBlocks Global/SelBlocks construct `function...endFunction`. If it's unclear, let's call the later _Selenese function, script function_ or _SelBlocksGlobal function_.

## script
Since Selenium and SeLite are not test-specific, SeLite documentation calls test cases or suites `scripts`. They're called `Selenese scripts` only when there's a need to differentiate them from Javascript (or other scripts). Some other terms are at [TestMethods](TestMethods) and [TestMethodsTheory](TestMethodsTheory).

Side note: [SelBlocks](https://addons.mozilla.org/en-US/firefox/addon/selenium-ide-sel-blocks/versions/) used to call Selenese _functions_ (defined by `function...endFunction`) _scripts_. (Originally they were defined by `script...endScript`.) However, SeLite refers to them as _functions_ and not _scripts_.

## Core extension
_Selenium Core extension_, or just _Core extension_, extends functionality available to Selenese scripts. It doesn't provide any visual interface. Generally it can be loaded from Selenium IDE or Webdriver. But even if a Core extension is for Selenium IDE only, it's called a _Core extension_ rather than _IDE extension_. Core extensions from SeLite are like that: they are Selenium IDE-specific and they don't work with Webdriver.

## IDE extension
_Selenium IDE extension_, or just _IDE extension_, extends GUI of Selenium IDE. It doesn't interfere with Selenese scripts. However, it may provide configuration or other interface used by a _Core extension_, and those can be packaged together in a Firefox add-on.

## Extension of Selenium IDE
This is either a _Core extension_ but for Selenium IDE only, or an _IDE extension_.

# Firefox chrome URLs for documentation and GUI
Selenium and SeLite GUI, some of their documentation and all their source, are accessible via special Firefox URLs that start with _chrome://_. (Those addresses don't work in other browsers).

Documentation presents such URLs as text _in italics_, but not as links. (That's because Firefox doesn't allow linking to them from the internet, neither from _file://_ based URLs.)

# Comments TODO move to ReportingIssues
By submitting a comment you agree that project administrator(s) can, without notice,

  * edit your comment, or
  * incorporate your comment into SeLite documentation, under the current or any future license of SeLite documentation, or
  * remove your comment.

# Details #
If you read TODO subfolder, or a separate listing page [Object Diagrams](https://code.google.com/p/selite/w/list?q=label:ObjectDiagram), you may want to see [DocumentationStandard](DocumentationStandard) > [Textual object diagrams](DocumentationStandard#textual-object-diagrams).