---
title: Settings API
layout: default
---

# Accessing SeLite Settings API #
Selenium Core extensions can access SeLite Settings-managed configuration as a Javascript code module using
```
Components.utils.import("chrome://selite-settings/content/SeLiteSettings.js", SeLiteSettings);
```
See the [API source](https://code.google.com/p/selite/source/browse/settings/src/chrome/content/SeLiteSettings.js).

# Defining a configuration module #
You can define a configuration module (schema) in a simple Javascript file (in UTF-8) by instantiating
  * subclasses of SeLiteSettings.Field, one instance per field
  * class SeLiteSettings.Module
The definition must be in a file available via
  * the filesystem (possibly a network drive), via either the path+name or via file:// url; or
  * a custom Firefox extension (at a chrome:// or resource:// url)

See a highlighted example of a schema definition [test\_settings\_module.js](https://code.google.com/p/selite/source/browse/settings/test_settings_module.js) (or download [its source](https://selite.googlecode.com/git/settings/test_settings_module.js)).

Whenever you update a module definition, you need to either
  * refresh it via url <i>chrome://selite-settings/content/tree.xul</i>, and restart any depending extensions (such as Selenium IDE); or
  * restart your Firefox.

# Registering and loading a module programatically #
Use SeLiteSettings.loadFromJavascript() to load or register & load a module programatically. You don't need this if you register the configuration file via [SettingsInterface](SettingsInterface).

# Reading values #
See class SeLiteSettings.Module and its methods
  * getFieldsOfSet() - primarily for modules that have associatesWithFolders==false
  * getFieldsDownToFolder() - only for modules that have associatesWithFolders==true

# Updating preferences #
See class SeLiteSettings.Field and its methods setValue(), addValue() and removeValue().