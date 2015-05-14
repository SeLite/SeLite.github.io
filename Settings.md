---
title: Settings Overview
layout: default
---
{% include links %}

# Summary #
[SeLite Settings](https://addons.mozilla.org/en-US/firefox/addon/selite-settings/versions/) allows easy custom configurations of Selenese scripts and [Core extensions][core extension]. See also [AddOns](AddOns).

# Background #
<!-- TODO Extract out: to AboutDocumentation? -->
Selenium IDE can be extensively customised via [Core extensions][core extension] (written in Javascript). This way you add new Selenese [commands][command] and related functionality. Many extensions are worth reusing (by yourself, within your team or publicly). But then they often need to be configured, e.g. in regard of

  * users or credentials
  * location of XML files (which an extension can pass to loadXmlVars and forXml from [SelBlocksGlobal](SelBlocksGlobal))
  * location of SQLite data files

# Configuration interface #
There was no convenient tool to

  * edit
  * validate
  * share and manage
such configurations.

Most [Core extensions][core extension] can work with

  * just a few basic field types
  * an option for custom validation of fields
  * simple user interface
    * no need for sophistication, since it won't be used frequently (as compared to other GUI parts of Selenium IDE)
    * done once for benefit of many
    * easier to use, share and manage than
      * editing Javascript files (possible typos..), or
      * using Firefox's special URL _about:config_, which would
        * hard-code preference names
        * allow only one configuration per Firefox profile. That is counter productive if you need to switch between configurations, since the profile contains the installed extensions, history etc. Copying the profile folders can lead to a mess.
  * an API to access the configured values

# Why not make your own #
You could create custom GUI as an IDE [extension of Selenium IDE] through XUL overlays (in XML). But

  * you shouldn't need to learn all that, especially because XUL is
    * different to HTML, and not of much use (unless you create visual extensions of Firefox)
    * difficult to debug
    * not well documented (some features)
  * from [script] developer's perspective, making extension(s) configurable is a not as productive/motivating as creating extensions themselves

# Functionality #
It defines API and GUI that simplify management and sharing of configurations. They
  * organise fields in modules and sets
  * use Firefox preferences and/or manifest files
  * allow fine-grain granularity
  * enable adding custom fields to existing configurations
  * populate default values
  * validate free-type values
  * give end users (e.g. testers) visual interface, which is nicer than Firefox URL _about:config_
  * present settings as they apply to a chosen folder

# API #
You can

  * define configuration schemas (called _modules_) and their fields
  * store any number of configuration sets (or their parts) per module in
    * plain text files (values manifests), and/or
    * Firefox profile and
      * edit them through a generic interface in a browser
      * choose one that is default (if the module allows multiple sets)
      * associate them to folders containing [suites][suite] (if the module allows to be associated with folders)
      * alternatively edit them access via url _about:config_
  * have your [Core extension] access the preferences via a Javascript code module (API)

Values manifests and associated sets work only in standalone Selenium IDE, but not in [SeleniumIDE](SeleniumIDE) > [auxiliary Selenium IDEs inside browser](SeleniumIDE#auxiliary-selenium-ides-inside-browser) (neither in Selenium IDE in browser sidebar).

# Modules and sets #
Modules are schemas/templates of user's configuration(s). They define each field and

  * its  name
  * its type
  * whether single-valued or multi-valued
  * default value(s)
  * custom validation (optional)

A field can be either

  * free-type - a string, an integer, a decimal
  * choice from a fixed list
  * boolean checkbox
  * file/folder picker

User's configuration(s) - values of the fields - are stored in _sets_. A module can have either

  * exactly one set, or
  * any number of sets; one of them can be selected as default

It also works without Selenium IDE and without manifests. So you can use it in any Firefox extension.

Test developers define configuration modules (schemas) in Javascript. See [SettingsFields](SettingsFields) and [SettingsAPI](SettingsAPI).

# Granularity #
Users can configure and override settings granularly via

  * profile-based configuration sets
    * through user interface
    * not shared with other team members
    * applied to [suites][suite] through _associations_ manifest file(s)
  * manifest file(s)
    * in plain text
    * in folder(s) on the [suite] file path
    * exist in two types
      * _values_ manifests
      * _associations_ manifests
    * can be shared, distributed and updated within a team via
      * a shared drive, or
      * source versioning system

For an example see [SettingsInterface](SettingsInterface). If accessing configuration from [Core extensions][core extension], see [SettingsAPI](SettingsAPI).