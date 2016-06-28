---
title: (Firefox extensions)
layout: default
---
{% include links %}
{::options auto_id_stripping="true" /}
* TOC
{:toc}

# Add-ons for running and development of Selenese scripts
Following add-ons are for development of Selenese [scripts][script]. For the easiest download get all add-ons of [SeLite Third Party collection](https://addons.mozilla.org/en-GB/firefox/collections/peter-kehl/selite-third-party/).

  * [Firebug](https://addons.mozilla.org/en-us/firefox/addon/firebug)
  * [FirePath](https://addons.mozilla.org/en-US/firefox/addon/firepath) For checking Selenese XPath selectors. Beware that Selenium IDE XPath is stricter than in FirePath. For example, boolean operators `and, or` are case-insensitive in Firepath, but case-sensitive in Selenium IDE. Sometimes changing `/` to `//` helps.
  * [XPath checker](https://addons.mozilla.org/en-US/firefox/addon/xpath-checker/) (beware that sometimes it doesn't pick up a change of the page)
  * [Stored Variables Viewer](https://addons.mozilla.org/en-US/firefox/addon/stored-variables-viewer-seleni/)
  * [Highlight Elements](https://addons.mozilla.org/en-us/firefox/addon/highlight-elements-selenium-id/)
  * [Log Search Bar](https://addons.mozilla.org/en-US/firefox/addon/log-search-bar-selenium-ide)
  * [Page Coverage](https://addons.mozilla.org/en-US/firefox/addon/page-coverage-selenium-ide)
  * [File Logging](https://addons.mozilla.org/en-US/firefox/addon/file-logging-selenium-ide/)
  * [Test Results](https://addons.mozilla.org/en-US/firefox/addon/test-results-selenium-ide/)
  * [SQLite Manager](https://addons.mozilla.org/en-US/firefox/addon/sqlite-manager)

If you develop [Core extensions][core extension] or SeLite frameworks (as per [GeneralFramework](GeneralFramework)) see also [DevelopmentTools](DevelopmentTools).

# Incompatible add-ons #
Don't use

  * original SelBlocks - it doesn't call Selenese functions across [cases][case]. Use [SelBlocksGlobal](SelBlocksGlobal) instead;
  * Flow Control, Sideflow, GoTo - they are incompatible with [SelBlocksGlobal](SelBlocksGlobal) (and also with SelBlocks).

# Other versions of Selenium IDE #
In order to use an old version of Selenium IDE 

  * [install it](https://addons.mozilla.org/en-US/firefox/addon/selenium-ide/versions/) and
  * disable its automatic updates in Firefox menu > Tools > Add-ons > Selenium IDE x.x.x > More > Automatic updates > Off.

If you'd like to use the current development version of Selenium IDE, see [InstallFromSource](InstallFromSource) > [Install Selenium IDE from source](InstallFromSource#install-selenium-ide-from-source).