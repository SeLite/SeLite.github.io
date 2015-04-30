---
title: DB-driven browser automation
layout: default
---

# Database-driven browser automation

## SeLite makes Selenium better
SeLite (Selenium+SQLite) is a family of extensions of [Selenium](SeleniumIDE). It
* enables user scripts to be more effective
* enhances development environment
* facilitates team work.

It serves for testing of web applications, and for other purposes, such as web-based administration or data mining/manipulation. It enables DB-driven navigation with [SQLite](http://www.sqlite.org/) (the [most widely deployed](http://www.sqlite.org/mostdeployed.html) SQL database).

## Data separation (in automated testing)
Some application errors cause incorrect data that doesn't show up on the immediate screens (or not at all during the same session). Such defects present themselves only on subsequent screens or even much later (through their knock-on effect). Having a test DB (in SQLite) isolated from the application's DB facilitates early detection of those bugs.

Separation allows validation of the data as presented by the application anytime later, rather than just right after the data was updated. So in addition to checking the immediate pages within the same test run, this also checks the application continually, taking any previous operations into account (including previous sessions). If the application shared its database with the test, such defects would be difficult or even impossible to detect.

See illustrations at [TestMethods](TestMethods) and details at [TestMethodsTheory](TestMethodsTheory).

## Benefits
SeLite enables the following in Selenium IDE
* reusable, structured and expressive scripts
 * [SelBlocksGlobal](SelBlocksGlobal): flow structures, blocks of Selenese commands (functions) re-used across test cases
 * [EnhancedSyntax](EnhancedSyntax) for shorter and cleaner scripts
 * [ExtraCommands](ExtraCommands) (e.g. random input generators)
* DB-driven operations
 * test database isolated from the application (see [TestMethods](TestMethods) and [HandlingData](HandlingData))
 * data access through high-level formulas (see [AddOns](AddOns) > DB Objects)
 * making and reverting database snapshots through [SettingsInterface](SettingsInterface) directly from Selenium IDE
* configuration of scripts - see [SettingsOverview](SettingsOverview)
 * having custom fields ([SettingsFields](SettingsFields))
 * managed through files ([SettingsManifests](SettingsManifests)) or via GUI ([SettingsInterface](SettingsInterface))
 * team-friendly: selectively shareable ([SettingsScope](SettingsScope))
* extra automation
 * [ExitConfirmationChecker](ExitConfirmationChecker) validates presence of confirmation when leaving unsubmitted forms
 * [AutoCheck](AutoCheck) detects webserver errors/warnings
 * [Run All Favorites](https://addons.mozilla.org/en-US/firefox/addon/selite-run-all-favorites/) executes multiple test suites
* productive environment
 * [Clipboard And Indent](https://addons.mozilla.org/en-US/firefox/addon/selite-clipboard-and-indent/) improves clibpoard usage and it supports indentation of commands
 * fast development cycle for custom Javascript functionality (via [BootstrapLoader](BootstrapLoader))
 * robust loading of extensions honouring dependancies (through [ExtensionSequencer](ExtensionSequencer))
 * see also [Selenium IDE](SeleniumIDE) for productivity tips.

# Install
SeLite is easy to install, with no server side (apart from the controlled web application itself). You need [Firefox](http://www.mozilla.org), [Selenium IDE](http://docs.seleniumhq.org/download/) and SeLite [AddOns](AddOns). You may want some [AddOnsThirdParty](AddOnsThirdParty).

## Frameworks
When using complex scripts you need a framework. Several come with SeLite and are listed at [AppsFrameworks](AppsFrameworks). Follow [InstallFramework](InstallFramework) and any framework-specific steps. Read [PackagedTests](PackagedTests) on how to run demos. See also [Customisation](index#customisation) below.

## Database
Scripts (whether for testing or not) can stores their data in [SQLite](http://www.sqlite.org/) database. When used for testing, test DB is initialised to a copy (or an export) of the web application's DB.
 * If the web application stores its data in [SQLite](http://www.sqlite.org/), test data lifecycle is streamlined. You can then use all buttons of [SettingsInterface](SettingsInterface). They snapshot application DB (into 'vanilla' DB). They (re)load test DB and/or application DB from that snapshot.
 * Otherwise import the app DB (from Postgres, MySQL and potentially other types) via customisable filters. Apply [DataImport](DataImport).

## Customisation
When customising or creating frameworks or components for Selenium IDE, following topics can assist you:
* [JavascriptEssential](JavascriptEssential)
* [CustomisingSelenese](CustomisingSelenese)
* [TestFramework](TestFramework)
* [JavascriptComplex](JavascriptComplex).

# Project details

## Compatibility
SeLite is compatible with Firefox 37 and Selenium IDE 2.9.0. It's platform-independent (and therefore it doesn't support Flash/Silverlight/ActiveX). Its part [SelBlocksGlobal](SelBlocksGlobal) is mostly forward compatible with SelBlocks 2.0.1.

It doesn't operate with other browsers, nor with Selenium WebDriver. Effectiveness and convenience of Selenium IDE together with SeLite can, however, outweigh WebDriver. For detailed comparison see [WhySeleniumIDE](WhySeleniumIDE).
<!-- Comment: Regarding Adobe Flash: I have't tried https://addons.mozilla.org/en-us/firefox/addon/flex-pilot-x (https://github.com/admc/flex-pilot-x - both last updated in May 2011!), neither https://code.google.com/p/sfapi/. They inject .swf, or they need to be compiled with the Flash application, respectively.-->

## License
The whole project is open source. Most [AddOns](AddOns) are under GNU LGPL 3.

## Moral support
The best ways you can support SeLite:
 * vote for [ThirdPartyIssues](ThirdPartyIssues)
 * tell others
 * create and share test frameworks