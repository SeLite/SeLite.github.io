---
title: (Firefox extensions)
layout: default
---
{% include links %}
{::options auto_id_stripping="true" /}

# Other versions of Selenium IDE #
In order to use an old version of Selenium IDE 

  * [install it](https://addons.mozilla.org/en-US/firefox/addon/selenium-ide/versions/) and
  * disable its automatic updates in Firefox menu > Tools > Add-ons > Selenium IDE x.x.x > More > Automatic updates > Off.

If you'd like to use the current development version of Selenium IDE, see [InstallFromSource](InstallFromSource) > [Install Selenium IDE from source](InstallFromSource#install-selenium-ide-from-source).

# Add-ons for development of Selenese [scripts][script]
{:#add-ons-for-development-of-selenese-scripts}

  * [Firebug](https://addons.mozilla.org/en-us/firefox/addon/firebug)
  * [FirePath](https://addons.mozilla.org/en-US/firefox/addon/firepath) For checking Selenese XPath selectors. Beware that sometimes an XPath works in FirePath, but not in Selenium IDE. Changing `/` to `//` within it seems to help.
  * [XPath checker](https://addons.mozilla.org/en-US/firefox/addon/xpath-checker/) (beware that sometimes it doesn't pick up a change of the page)
  * [Stored Variables Viewer](https://addons.mozilla.org/en-US/firefox/addon/stored-variables-viewer-seleni/)
  * [Highlight Elements](https://addons.mozilla.org/en-us/firefox/addon/highlight-elements-selenium-id/)
  * [Log Search Bar](https://addons.mozilla.org/en-US/firefox/addon/log-search-bar-selenium-ide)
  * [Log Search Bar](https://addons.mozilla.org/en-US/firefox/addon/log-search-bar-selenium-ide/)
  * [Page Coverage](https://addons.mozilla.org/en-US/firefox/addon/page-coverage-selenium-ide)
  * [Favorites (favorite suites in Selenium IDE)](https://addons.mozilla.org/en-US/firefox/addon/favorites-selenium-ide/)
  * [File Logging](https://addons.mozilla.org/en-US/firefox/addon/file-logging-selenium-ide/)
  * [SQLite Manager](https://addons.mozilla.org/en-US/firefox/addon/sqlite-manager)

If you develop [Core extensions][core extension] or SeLite frameworks (as per [GeneralFramework](GeneralFramework)) see also [DevelopmentTools](DevelopmentTools).

# Incompatible add-ons #
Don't use

  * original SelBlocks - it doesn't call Selenese functions across [cases][case]. Use [SelBlocksGlobal](SelBlocksGlobal) instead;
  * Flow Control, Sideflow, GoTo - they are incompatible with [SelBlocksGlobal](SelBlocksGlobal) (and also with SelBlocks).