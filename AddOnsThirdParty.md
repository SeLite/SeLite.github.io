---
title: Third party Firefox extensions
layout: default
---

# Selenium IDE #
You need current [Selenium IDE](http://seleniumhq.org/download) (or version specified at [Overview](./) > [Status and compatibility](./#status-and-compatibility). If a new version of Selenium IDE is incompatible with SeLite then
  * try one of the [previous versions](http://release.seleniumhq.org/selenium-ide) and
  * disable automatic updates in Firefox menu > Tools > Add-ons > Selenium IDE x.x.x > More > Automatic updates > Off.
If you'd like to use current development version of Selenium IDE, see (InstallFromSource)[InstallFromSource] > [Install Selenium IDE from source](InstallFromSource#install-selenium-ide-from-source).

# Add-ons for development of tests and test components #
  * [Firebug](https://addons.mozilla.org/en-us/firefox/addon/firebug)
  * [FirePath](https://addons.mozilla.org/en-US/firefox/addon/firepath) For checking Selenese XPath selectors. Beware that sometimes an XPath works in FirePath, but not in Selenium IDE. Changing / to // within it seems to help.
  * [XPath checker](https://addons.mozilla.org/en-US/firefox/addon/xpath-checker/) (beware that it sometimes doesn't pick up a change of the page)
  * [Stored Variables Viewer](https://addons.mozilla.org/en-US/firefox/addon/stored-variables-viewer-seleni/)
  * [Highlight Elements](https://addons.mozilla.org/en-us/firefox/addon/highlight-elements-selenium-id/)
  * [Log Search Bar](https://addons.mozilla.org/en-US/firefox/addon/log-search-bar-selenium-ide)
  * [Log Search Bar](https://addons.mozilla.org/en-US/firefox/addon/log-search-bar-selenium-ide/)
  * [Page Coverage](https://addons.mozilla.org/en-US/firefox/addon/page-coverage-selenium-ide)
  * [Favorites (favorite test suites in Selenium IDE)](https://addons.mozilla.org/en-US/firefox/addon/favorites-selenium-ide/)
  * [File Logging](https://addons.mozilla.org/en-US/firefox/addon/file-logging-selenium-ide/)
  * [SQLite Manager](https://addons.mozilla.org/en-US/firefox/addon/sqlite-manager)

See also [DevelopmentTools](DevelopmentTools).

# Incompatible add-ons #
Don't use
  * original SelBlocks - it doesn't call functions across test cases. Use [SelBlocksGlobal](SelBlocksGlobal) instead
  * Flow Control, Sideflow, GoTo - they are incompatible with [SelBlocksGlobal](SelBlocksGlobal) (and also with SelBlocks)