---
title: Settings API
layout: default
---
{% include links %}

# SeLite Settings API #
Operate SeLite Settings-managed configurations through API. See [its source](https://code.google.com/p/selite/source/browse/settings/src/chrome/content/SeLiteSettings.js). In [Core extensions][core extension] access it through object `SeLiteSettings`. In other scopes, e.g. Selenium [IDE extensions][ide extension] or Javascript code modules (as per [JavascriptComplex](JavascriptComplex) > [Javascript code modules](JavascriptComplex#javascript-code-modules)), call

```
Components.utils.import("chrome://selite-settings/content/SeLiteSettings.js");
```

# Defining a configuration module
Define a configuration module (schema) in a Javascript file (in UTF-8). See [test\_settings\_module.js](https://github.com/selite/selite/blob/master/settings/test_settings_module.js) as an example. Instantiate

  * subclasses of `SeLiteSettings.Field`, one instance per field
  * class `SeLiteSettings.Module`, one instance per module

The definition must be

  * on the filesystem (possibly a network drive), referenced by its path or _file://_ URL; or
  * in a custom Firefox extension (at [_chrome://_ URL](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) or _resource://_ URL)

Whenever you update a module definition, you need to either

  * refresh it via url _chrome://selite-settings/content/tree.xul_, and restart any depending extensions (such as Selenium IDE); or
  * restart Firefox.

# Registering and loading a module programatically #
Use `SeLiteSettings.loadFromJavascript()` to load or register & load a module programatically. You don't need this if you register the configuration file via [SettingsInterface](SettingsInterface).

# Reading values #
See class SeLiteSettings.Module and its methods

  * `getFieldsOfSet()` - primarily for modules that have `associatesWithFolders==false`
  * `getFieldsDownToFolder()` - only for modules that have `associatesWithFolders==true`

# Updating preferences #
See class `SeLiteSettings.Field` and its methods `setValue(), addValue()` and `removeValue()`.