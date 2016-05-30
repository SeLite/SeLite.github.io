---
title: (DB-driven browser automation)
layout: default
---
{% include links %}

# Hands-on browser automation

## SeLite makes Selenium better
SeLite is for automated browser navigation and testing. It extends Selenium. It

 * improves Selenium (API, syntax and visual interface)
 * enables reuse
 * facilitates team work

SeLite enables DB-driven navigation with SQLite (the [most widely deployed](http://www.sqlite.org/mostdeployed.html) SQL database).

## Benefits
SeLite enables the following in Selenium IDE

* reusable, structured and expressive scripts
  * [SelBlocksGlobal](SelBlocksGlobal): flow structures, blocks of Selenese commands (functions) re-used across Selenese [cases][case]
  * [EnhancedSelenese](EnhancedSelenese): enhanced syntax for shorter and cleaner scripts
  * [ExtraCommands](ExtraCommands) (e.g. random input generators)
* DB-driven operations
  * test database isolated from the application (see [TestMethods](TestMethods) and [HandlingData](HandlingData))
  * data access through high-level formulas (see [AddOns](AddOns) > DB Objects)
  * making and reverting database snapshots through [SettingsInterface](SettingsInterface) directly from Selenium IDE
* configuration of scripts - see [Settings](Settings)
  * having custom fields ([SettingsFields](SettingsFields))
  * managed through files ([SettingsManifests](SettingsManifests)) or via GUI ([SettingsInterface](SettingsInterface))
  * team-friendly: selectively shareable ([SettingsScope](SettingsScope))
* extra automation
  * [ExitConfirmationChecker](ExitConfirmationChecker) validates presence of confirmation when leaving unsubmitted forms
  * [AutoCheck](AutoCheck) detects webserver errors/warnings
  * [Run All Favorites](https://addons.mozilla.org/en-US/firefox/addon/selite-run-all-favorites/) executes multiple Selenese [suites][suite]
* productive environment
  * [Hands-on GUI](SeleniumIDE#hands-on-gui) enhances Selenium IDE visual interface. It enables editing commands in-place (in the list itself). It makes Selenium IDE more intuitive.
  * [Clipboard And Indent](https://addons.mozilla.org/en-US/firefox/addon/selite-clipboard-and-indent/) improves operation of clibpoard. It supports indentation of commands.
  * fast development cycle for custom Javascript functionality (via [Bootstrap](Bootstrap))
  * robust loading of extensions honouring dependancies (through [ExtensionSequencer](ExtensionSequencer))
  * [Preview](Preview) presents custom reports and forms. User can preview and confirm next actions.
  * see also [Selenium IDE](SeleniumIDE) for productivity tips.

# Install
SeLite is easy to install, with no server side (apart from the controlled web application itself). You need [Firefox](http://www.mozilla.org) and [AddOns](AddOns) (Selenium IDE and add-ons of SeLite family). You may want some [AddOnsThirdParty](AddOnsThirdParty).

## Frameworks
When using complex scripts you need a framework. Several come with SeLite and are listed at [AppsFrameworks](AppsFrameworks). Follow [InstallFramework](InstallFramework) and any framework-specific steps. Read [PackagedScripts](PackagedScripts) on how to run demos. See also [Customisation](index#customisation) below.

## Database
Scripts (whether for testing or not) can store their data in [SQLite](http://www.sqlite.org/) database. When used for testing, test DB is initialised to a copy (or an export) of the web application's DB.

 * If the web application stores its data in [SQLite](http://www.sqlite.org/), test data lifecycle is streamlined. You can then use all buttons of [SettingsInterface](SettingsInterface). They snapshot application DB (into 'vanilla' DB). They (re)load test DB and/or application DB from that snapshot.
 * Otherwise import the app DB (from Postgres, MySQL and potentially other types) via customisable filters. Apply [DataImport](DataImport).

## Data separation (in testing)
Some application errors cause incorrect data that doesn't show up on the immediate screens (or not at all during the same session). Such defects present themselves only on subsequent screens or even much later (through their knock-on effect). Having a test DB (in SQLite) isolated from the application's DB facilitates early detection of those bugs.

Separation allows validation of the data as presented by the application anytime later, rather than just right after the data was updated. So in addition to checking the immediate pages within the same test run, this also checks the application continually, taking any previous operations into account (including previous sessions). If the application shared its database with the test, such defects would be difficult or even impossible to detect.

See illustrations at [TestMethods](TestMethods) and details at [TestMethodsTheory](TestMethodsTheory).

## Customisation
When customising or creating frameworks or components for Selenium IDE, following topics can assist you:

* [JavascriptEssential](JavascriptEssential)
* [CustomisingSelenese](CustomisingSelenese)
* [GeneralFramework](GeneralFramework)
* [JavascriptComplex](JavascriptComplex).

# Project details

## Standards
 * [strict Javascript](JavascriptEssential#strict-javascript)
 * coding standard ([JavascriptEssential](JavascriptEssential), [JavascriptComplex](JavascriptComplex) and [JavascriptSpecial](JavascriptSpecial))
 * [AboutDocumentation](AboutDocumentation) and [DocumentationStandard](DocumentationStandard)
 * validated by [PackagedScripts](PackagedScripts)
 * clear instructions on [TroubleShooting](TroubleShooting) and [ReportingIssues](ReportingIssues)
 * code on GitHub: [SeLite](https://github.com/SeLite/SeLite), [SelBlocksGlobal](https://github.com/SeLite/SelBlocksGlobal) and [documentation](https://github.com/SeLite/SeLite.github.io)
 * [AddOns](AddOns) are verified by Mozilla

## Compatibility
SeLite is compatible with Firefox 45 and Selenium IDE 2.9.1. It's platform-independent. It doesn't support Flash/Silverlight/ActiveX. Its part [SelBlocksGlobal](SelBlocksGlobal) is mostly forward compatible with SelBlocks 2.0.1.<!-- Comment: Regarding Adobe Flash: I have't tried https://addons.mozilla.org/en-us/firefox/addon/flex-pilot-x (https://github.com/admc/flex-pilot-x - both last updated in May 2011!), neither https://code.google.com/p/sfapi/. They inject .swf, or they need to be compiled with the Flash application, respectively.-->

It doesn't operate with browsers other than Firefox, neither with Selenium WebDriver (nor with Selenium RC). Effectiveness and convenience of Selenium IDE together with SeLite can, however, outweigh WebDriver. For detailed comparison see [WhySeleniumIDE](WhySeleniumIDE).

Subscribe to [XML RSS feed](http://www.feed43.com/selite-compatibility.xml) on compatibility.<!-- Don't include that in the header of this page. It's the landing page, hence we want its RSS to reflect changes anywhere in the documentation. -->

## License
SeLite is fully open source. Most [AddOns](AddOns) are under GNU LGPL 3.

## Moral support
The best ways you can support SeLite:

 * vote for [ThirdPartyIssues](ThirdPartyIssues)
 * tell others
 * create and share frameworks.