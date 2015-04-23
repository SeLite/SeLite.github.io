---
title: Install a framework
layout: default
---

# Scope and downloads #
[Edit or review through GUI](#edit-or-review-through-gui)
This assumes that you've set up your web application and that there is an SeLite framework for it. You'll need packages mentioned at [Home](./) > [Install](./#install). However, if you haven't downloaded SeLite [AddOns](AddOns) yet, it may be faster to install them from source (which you'll need anyway) as per [InstallFromSource](InstallFromSource) > [Get the source](InstallFromSource#get-the-source) and [Install add-ons from source](InstallFromSource#install-add-ons-from-source).

Regardless of how you've installed [AddOns](AddOns), download SeLite source to get the frameworks: follow [InstallFromSource](InstallFromSource) > [Get the source](InstallFromSource#get-the-source).

If your web application uses SQLite, you'll get full functionality and smoother test data life cycle. You'll be able to copy/restore _appDB_, _testDB_ and _vanillaDB_ from within SeLite (via [SettingsInterface](SettingsInterface)). Otherwise you need to apply [DataImport](DataImport).

The following instructions are generic. For any application-specific steps see documentation of the respective framework.

# Configuration #
## Areas, methods and stages of configuration ##
You need to configure two main areas
  * what framework (or any other bootstrapped extensions) to load
  * common and framework/extension-specific settings

There are two methods for this. The simpler one manages it through text file(s), as per [SettingsManifests](SettingsManifests) > ['Values' manifests](SettingsManifests#values-manifests). Such configuration is easier to share and replicate than one administered through GUI. It also involves less steps and only one stage. You can still use GUI to review effect of 'values' manifests (though it involves some extra steps).

An alternative method is Firefox profile-based configuration set(s), controlled through GUI. It can also override any 'values' manifests. It's done through [SettingsManifests](SettingsManifests) > ['Associations' manifests](SettingsManifests#associations-manifests). However, using GUI (for any part of configuration) usually requires the whole configuration to be in two stages.

## Configure SeLite to load the framework ##
### Through 'values' manifests ###
This is already done for the tests that come with SeLite frameworks. If you're creating tests with any of those frameworks, you can copy its 'values' manifest _SeLiteSettingsValues.txt_. Otherwise create it as a plain text file. Either way, you then need to adjust/enter value of field _extensions.selite-settings.common.bootstrappedCoreExtensions_, so it points to location of the framework Javascript file. See also [SettingsManifests](SettingsManifests) > ['Values' manifests](SettingsManifests#values-manifests) and [SettingsManifests](SettingsManifests) > [Literals for special values](SettingsManifests#literals-for-special-values).

### Through GUI ###
<!-- @TODO eliminate or Move to SettingsInterface? -->
You can configure that via GUI as per [SettingsInterface](SettingsInterface):
  1. Visit [_chrome://_ URL](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) _chrome://selite-settings/content/tree.xul?module=extensions.selite-settings.common_
  1. create a new set
  1. if you're using just one test configuration, make it default (click at 'Default' next to its name)
  1. select 'Add a new value' next to _bootstrapCoreExtensions_. Point it to where you downloaded the framework JS file.

If you're going to use several test configurations, then create one set for each configuration, but don't make any of them default. Then connect the sets with folders of your test suite(s) through 'associations' manifest as per [SettingsManifests](SettingsManifests) > ['Associations' manifests](SettingsManifests#associations-manifests).

## Edit or review configuration of the framework ##
### Edit through 'values' manifests ###
Add values for applicable following fields and for any custom fields/keys to _SeLiteSettingsValues.txt_ from above
```
extensions.selite-settings.common.testDB
extensions.selite-settings.common.appDB
extensions.selite-settings.common.vanillaDB
extensions.selite-settings.common.webRoot
extensions.selite-settings.common.tablePrefix
extensions.selite-settings.common.roles:abc pqrs
extensions.selite-settings.common.roles:def ijkl
...
```
See descriptions below.

### Edit or review through GUI ###
This has a dual purpose: to maintain profile-based configuration set(s), and to review effect of configuration set(s) or 'values' manifest(s). If you want to use configuration set(s), create them if you haven't done so (see above).

Before you use GUI, you need to load the bootstrapped framework. Start Selenium IDE and run any single test command, e.g. <i>getEval | true</i>. That has effect only for the current run of Firefox (so next time you start the browser you'll need to repeat it in order to use GUI). Then you can configure roles and any framework-specific fields.

If you'd like to review configuration, open [_chrome://_ URL](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) _chrome://selite-settings/content/tree.xul?selectFolder_ (or _chrome://selite-settings/content/tree.xul?folder=/full/path/to/folder_).

If you'd like to edit profile-based configuration set(s), open _chrome://selite-settings/content/tree.xul_. Then follow [SettingsInterface](SettingsInterface) and edit the following fields (if they apply to your framework):
  * Enter usernames for _roles_ that you need.
  * Point the configuration set at your app/test/vanilla SQLite files. You need at least _testDB_ to get benefits of [Home](./) > [Advantages of test data separation](./#advantages-of-test-data-separation). _appDB_ and _vanilla_ make sense only if the application data is in SQLite.
    * For _appDB_ select the SQLite file which is used by your application instance. (The file often has extension other than .sqlite, e.g. .db or even .php.)
    * _testDB_ is for the test scripts. _vanillaDB_ will serve as a snapshot of _appDB_, so that you can revert _appDB_ and _testDB_ to it. Enter some new filenames (in a location where your account can create files).
    * If you haven't got existing _testDB_ and _vanillaDB_, in Selenium IDE click at button ![Reload Vanilla and Test](https://raw.githubusercontent.com/selite/main/master/settings/src/chrome/skin/classic/reload_vanilla_and_test.png). That reloads vanillaDB and testDB from appDB. (See [SettingsInterface](SettingsInterface)).
  * Fill in _webRoot_ (it may end with a slash or not). Your tests can access it via _SeLiteSettings.webRoot()_. (This is a workaround for Selenium IDE issue ['Base URL Should Allow Path'](http://code.google.com/p/selenium/issues/detail?id=3116). Please, vote for it and also for other [ThirdPartyIssues](ThirdPartyIssues).)
  * Fill in _tablePrefix_.
  * Open the URL of the installation, log in with account(s) that you entered for role(s) above and make Firefox save your password(s). (That's for [SettingsLogins](SettingsLogins).)
  * You may want to activate [AutoCheck](AutoCheck) to detect notices/warnings/errors. Currently that works out-of-the-box for PHP only.

# Run tests #
Unless you are using GUI to maintain or review configuration, you can run test cases or suites right after you start Selenium IDE. You don't need to run any single Selenese command first.

Locate, open and run a test suite as per [PackagedTests](PackagedTests).

## No hot switching between frameworks ##
If you have two or more frameworks, don't switch between them during the same Firefox run. You need to restart Firefox (not just Selenium IDE). Reasons:
  * Frameworks can remove configuration fields that are not relevant to them, or they add new ones. Such fields stay removed/added during the rest of Firefox run, even after you switch to a different framework.
  * If you start with one framework, switch to another one and then back to the first one (all within the same Firefox run), the first framework won't be re-applied unless its file was modified (as per [BootstrapLoader](BootstrapLoader) > [Switching between files](BootstrapLoader#switching-between-files)).
  * Frameworks usually benefit from _setTestDbKeeper()_, but only one framework (or extension) can invoke it. See [TestFrameworks](TestFrameworks) > [Preserving special values in test DB](TestFrameworks#preserving-special-values-in-test-DB).