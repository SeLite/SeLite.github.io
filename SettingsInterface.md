---
layout: default
---
{% include links %}
* TOC
{:toc}

# Accessing the interface of SeLiteSettings #
Two most frequent use cases are available in Selenium IDE menu > Options. They link to `tree.xul` and `tree.xul?selectFolder` (see below). Changes are immediate: if you change a field's value, or delete a set, it's saved right then.

SeLiteSettings' configuration interface is within Firefox at {{chromeUrl}}s. The URLs are

| For [**Managing mode**](#managing-mode): | |
| _chrome://selite-settings/content/tree.xul_                                   | to manage Firefox profile-based set(s) for all registered modules |
| _chrome://selite-settings/content/tree.xul?module=full-module-name_           | to manage set(s) for a given module |
| _chrome://selite-settings/content/tree.xul?prefix=prefix-of-full-module-name_ | to manage set(s) of all modules whose name starts with given prefix |
| For [**Reviewing mode**](#reviewing-mode): | |
| _chrome://selite-settings/content/tree.xul?selectFolder_                      | to select a folder for which to review any fields |
| _chrome://selite-settings/content/tree.xul?folder=/full/path/to/folder_       | to review effective configuration for [suites][suite] in that folder, based on any applicable sets or manifests as per [SettingsScope](SettingsScope) |
| _chrome://selite-settings/content/tree.xul?register_ | to register a module manually (from its javascript file). You need this only if you design or import a custom configuration module that is not packaged as an [extension of Selenium IDE]. Alternatively you may use your own [Core extension] and [SettingsAPI](SettingsAPI) to register your module automatically. |
{: .table}

Per-folder view colours fields based on where the value(s) come from:

 * module definition (field default)
 * Firefox profile-based configuration set
 * {{valuesManifest}}

# Navigation
Hover the mouse over a field row for a  mouseover tip with a field description.

## Managing mode
In managing mode (e.g. at {{chromeUrl}} _chrome://selite-settings/content/tree.xul_), clicking at a cell in column

   * 'Default', at set level, toggles the set default.
   * 'True', 'Value' at a field/field entry level toggles the boolean value, or edits the value, respectively.
   * 'Null/Undefine' at a field/field entry level toggles the value first to `null`, then to `undefined`.
   * 'Value' or 'Add/Delete' at a field/field entry level, for a file-based field (e.g. `testDB` or `bootstrappedCoreExtensions`) opens a file picker dialogue.

E.g. to create a new [set]:

  1. Visit {{chromeUrl}} _chrome://selite-settings/content/tree.xul_.
  2. Press 'Add a new set' (for `extensions.selite-settings.common`, or for a custom module).
  3. Make the set default (click at 'Default' column next to its name). However, if you're going to use several script configurations, then create a [set] for each, but don't make any set default. Then connect the sets to your [suite] folders as per {{navAssociationsManifests}}.
  4. Enter values.

## Reviewing mode
In per-folder reviewing mode (e.g. at {{chromeUrl}} _chrome://selite-settings/content/tree.xul?folder=/full/path/to/folder_), clicking at 'Manifest/Definition' opens a definition of the module for that field, or a {{valuesManifest}} where the value comes from (if any). 'Set' column indicates the [set] where the value came from (if from a set).

# Reloading databases
SeLiteSettings adds three buttons to Selenium IDE. They re-load one or two of [script DB], [app DB] or [vanilla DB] (as per the table below). These buttons do the full job on their own only if your web app uses SQLite as its DB. Otherwise apply [DataImport](DataImport).

| **Selenium IDE button** | **Source DB** | **Target DB** | **Extra target DB** |
| ![reload test DB](https://raw.githubusercontent.com/selite/selite/master/settings/src/chrome/skin/classic/reload_test.png) | App | Test | |
| ![reload vanilla and test DB](https://raw.githubusercontent.com/selite/selite/master/settings/src/chrome/skin/classic/reload_vanilla_and_test.png) | App | Test | Vanilla |
| ![reload app and test DB](https://raw.githubusercontent.com/selite/selite/master/settings/src/chrome/skin/classic/reload_app_and_test.png) | Vanilla | Test | App |
{: .table}

You should pause [scripts][script] while using these buttons, otherwise the [script] or application may modifying their DB files. Beware of background web processes (Ajax or CRON) - wait until they finish. Otherwise you may need to stop the application (e.g. by shutting down Tomcat/JBoss, Apache or WEBrick). If the DB file is on a network filesystem, it may not lock properly.

## Permissions
Reloading databases requires your local account (which runs Firefox) to have access to delete the web [app DB] file and to create new files in the web app DB folder. That works when the app runs in  locally and you started it yourself, or when it is under your home folder. Otherwise your account needs to have the access granted (e.g. via Linux/Mac OS groups or SeLinux `setfacl` - see [DrupalFramework](DrupalFramework)).

# Entering `'null'` or `'undefined'`
This is unlikely, but possible. If a freetype field has a non-null defined value (possibly empty string), then you can type in string literals `'null'` or `'undefined'` (within apostrophes or quotes) and they will be accepted as those strings. They won't get converted to Javascript null or undefined. Use a special column 'Null/Undefine' for that.

If a freetype field is Javascript `null` or `undefined`, you can click at its cell and change it. If you you don't change it and you hit Enter, that leaves the field as Javascript `null` or `undefined`, rather than a string literal `'null'` or `'undefined'`. So, if  you'd like to enter a string literal `'null'` or `'undefined'`, enter something else first and then change it to `'null'` or `'undefined'` again.