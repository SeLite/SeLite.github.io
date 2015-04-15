---
title: Test framework
layout: default
---


# What a test framework is #
A test framework is on optional layer, that encapsulates low-level application-specific test components. Those can be Javascript structures and Selenese functions.

Javascript structures are Selenium Core extensions that define custom functionality for the tests. They can be optionally packaged as Firefox extensions (public or proprietary). They can contain:
  * Object-oriented descriptions of DB schema. Those define DB tables and their relationships.
  * Definitions of DB formulas. Those let you select data without writing SQL queries.
  * functions and object shorthands
  * Selenese commands


Selenese functions (blocks of Selenese commands) can be shared between test cases and test suites (via [SelBlocksGlobal](SelBlocksGlobal)). The framework consists of library-like test cases, which only contain Selenese functions and no commands outside those functions. Those test cases are shared by all test suites, which run the actual tests.

See [DrupalFramework](DrupalFramework), [its source](https://code.google.com/p/selite/source/browse/drupal) and other [AppsFrameworks](AppsFrameworks).

## Loading a test framework ##
A test framework is loaded as a Selenium Core extension. The easiest way to load frameworks is through [BootstrapLoader](BootstrapLoader). Alternatively you can make it a Firefox extension (with use of [ExtensionSequencer](ExtensionSequencer)), either packaged in an .xpi file or loaded through a proxy file (see [DevelopmentTools](DevelopmentTools)).

## Configuring and loading the data ##
You can manage configuration through [Settings](SettingsOverview). Configuration sets can associate with tests on per-test suite folder basis. A framework needs to know the test suite folder, so that it can resolve its configuration (which determines the test DB). A framework shouldn't pre-load any data when it's activated, because test suite folder may not be known yet. If you need pre-loading any data, do that from a test suite folder change handler, which gets triggered once you open (or save) a test suite.

# Extending a test framework #
If you need to extend an existing framework, you may want to do it in a separate file. That makes it easy to receive updates from SeLite or within your team.

However, standard frameworks get loaded by [BootstrapLoader](BootstrapLoader), which doesn't guarantee order of loading multiple files. So one framework file can't robustly and easily intercept/replace functions defined in another file of the same framework (since the order of loading multiple files may work in one Firefox profile, but not in another). If you need that, replace the existing framework rather than extend it.

All files that define parts of the same framework declare the same namespace object near their top. If the object doesn't exist yet, they also create it, with any essential fields (using the same code in all those files).

For many frameworks the only essential field of the namespace object is _db_. Then all files of such framework start with a block like:
```javascript

"use strict";

/** @type{object} A namespace-like object in the global scope.*/
var Drupal;
if( Drupal===undefined ) {
Drupal= {
/** @type {SeLiteData.Db}*/
db: new SeLiteData.Db( SeLiteData.getStorageFromSettings() )
};
}
```

## Adding custom roles ##
If you'd like to add custom roles, use e.g.
```javascript

var commonSettings= SeLiteSettings.loadFromJavascript( 'extensions.selite-settings.common' );
commonSettings.getField( 'roles' ).addKeys( ['second-level-admin', 'auditor', 'contributor'] );
```

# Creating a new framework #
Look at source of existing frameworks - see [AppsFrameworks](AppsFrameworks).

## Preserving special values in test DB ##
You may want your tests to save special values in their DB. E.g. your framework could create or update users, generate random passwords for them and save those passwords in plain text (rather than encrypted), so that further runs could log in as those users.

When reloading test DB (via either button), you don't want to override such special values from production/vanilla. SeLiteSettings can preserve them. Your framework needs to call _SeLiteSettings.setTestDbKeeper()_ with an instance _SeLiteSettings.TestDbKeeper_.Columns (or with an instance of a custom subclass of _SeLiteSettings.TestDbKeeper_).

## Limitations ##
  * Call _SeLiteSettings.setTestDbKeeper()_ even if you don't use testDbKeeper - then call it as _SeLiteSettings.setTestDbKeeper(null);_ (This is needed by GUI buttons that reload test/app/vanilla DB.)
  * Give your framework file a (fairly) unique name.
  * If you've already loaded your test framework (i.e. you've run a Selenese command in a test suite that uses that framework) and then you modify its call to _SeLiteSettings.setTestDbKeeper()_, the new call won't have effect until you restart Firefox.

The reason for those limitations is in code of _SeLiteSettings.setTestDbKeeper()_.

### One stage GUI configuration ###
<a href='Hidden comment: @TODO move to a page on its own: CreateExtensions '></a>This is only enabled for Selenium Core extensions that come with SeLite, but not for test frameworks. It enables the extension to add custom configuration fields, or to add custom options for existing configuration fields. Those new fields or new options (_keys_) are available in [SettingsInterface](SettingsInterface) immediately after start of Firefox. That is different to fields or options added by test frameworks, loaded through [BootstrapLoader](BootstrapLoader), which have effect only after running the first Selenese command.

It requires that the extension is packaged as a Firefox add-on, rather than loaded through [BootstrapLoader](BootstrapLoader). The add-on has to be installed from an .xpi package, or through a proxy file as per [InstallFromSource](InstallFromSource). It has to have _SeLiteExtensionSequencerManifest.js_ with _preActivate_ handler, where it adds any custom fields. See source of e.g. _selite/auto-check/src/chrome/content/SeLiteExtensionSequencerManifest.js_.