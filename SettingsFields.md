---
title: SettingsFields
layout: default
---

# Summary #
Fields can be

  * free-type: String, Int, Decimal
  * Bool
  * choices: String, Int, Decimal
  * File (including Folder or SQLite)
  * FixedMap (a freetype map with fixed keys), which serves for [SettingsLogins](SettingsLogins) > [Roles hard coded in Settings module configuration](SettingsLogins#roles-hard-coded-in-settings-module-configuration)

All fields can be single-valued or multi-valued, except for

  * Bool and SQLite, which are always single-valued and
  * FixedMap, which is always multi-valued

They are stored in Firefox preferences (on top of values manifests). Therefore

  * there are no custom composite field types
  * decimal numbers are stored as s trings (but the API works with them as numbers - so they may get rounded, depending on the CPU float precision)
  * Int and Decimal fields don't accept NaN (not-a-number) neither infinite values

# Special values #

## Undefined ##
A field of any type can be 'undefined' (unless it was instantiated with value of parameter requireAndPopulate other than false). If the module is associated with a folder, and the field is undefined in that associated set, it gets inherited from

  * a set associated with a higher folder (the nearest one that defines the value), or
  * from the default set (if any such set, and if the field is defined there), or
  * from the nearest applicable values manifest, or
  * from the field default value (from the module definition).

See also [SettingsScope](SettingsScope).

## Present and empty ##
'Present and empty' is only allowed for multivalued fields (choice or non-choice). It means that a multi-valued field has no entry, and it should be stored and presented as such (rather than inheriting values from other applicable sets or manifests). It can be present in values manifests or preferences. In values manifests it's represented by string _SELITE\_VALUE\_PRESENT_.

## Null ##
Most _Field_ types can be configured to allow null. It's possible only for single-valued fields (boolean/int/string, file, choice).  Single-valued choice field itself can have value _null_ when no choice is selected, but none of its choices can have _null_ value. Use literal _SELITE\_NULL_ to represent null in 'values' manifests (as per [SettingsManifests](SettingsManifests) > [Literals for special values](SettingsManifests#literals-for-special-values)). You can specify Javascript's _null_ as a field default value in the module definition.

Values of multi-valued fields can't be _null_ (see below); but they can be 'undefined' or 'empty but present'. The whole multi-valued field can't be _null_, either. (Explanation: _null_ will never be supported for items of multi-valued/choice fields. For practicality the API presents and stores both values of multi-valued fields and keys of choice fields as names of Javascript object properties (i.e. object keys). That can't handle _null_ well. Javascript transforms it into string 'null', which then couldn't be distinguished from proper string value _null_. E.g. <em>var o={}; o[null]= 1; Object.keys(o)[0]==='null'</em> evaluates to true.)

# Default values #
SeLite Settings populates default values for some fields. It happens when

  * you create a new set (or when a module, which doesn't allow multiple sets, gets registered - then it creates its only set), or
  * when there is a new definition of the module, that adds new field(s)
  * and module.associatesWithFolders==false and field.requireAndPopulate==true

If module.associatesWithFolders==true, we want the ability to inherit values from sets/values manifests from higher folders. Therefore it doesn't populate default value for any fields of such modules.

If the schema defines a field with defaultValue===undefined, then it's not populated in the set(s).

# File fields #
Values of _Field.File_ fields stored in configuration sets are full (absolute) paths to files (or folders). So they may not work for other users. Also, if you copy your Firefox profile folder to a system that uses a different folder separator (between Windows and Max OS/Linux), these fields won't work.

However, you can use 'values' manifests to assign _Field.File_ fields relative to the manifest's folder. Also, such paths will work on either system (Windows or Mac OS/Linux). See [SettingsManifests](SettingsManifests) > [Literals for special values](SettingsManifests#literals-for-special-values).

## Selecting files in GUI ##
When selecting a value (a file) for fields of class _Field.File_ defined with 'saveFile' option, the file selector dialog behaves as if you were saving a file. So if you select an existing file, it will confirm with you whether you want to overwrite it. However, the file won't be overwritten until the field is actually used for that purpose. Examples of such fields are ones for testDB and vanillaDB (see [SettingsInterface](SettingsInterface) > [Reloading databases](SettingsInterface#reloading-databases)).

Whether a _Field.File_ or _Field.SQLite_ field is marked as 'saveFile' or not, the framework can overwrite such a file. However, 'saveFile' makes it possible to specify names of files that don't exist yet. Without that flag a file needs to exist first before you can select it for the field.

# Standard and custom validation #
Standard validation

  * _Field.Int_ accepts whole integer numbers (negative/positive/zero)
  * multi-valued fields refuse duplicate values
  * _Field.Choice_ accepts keys only from within the choice list
  * _Field.File_ accepts files only with an extension from the given list (if specified)

If you provide a custom validation function, that is used in addition to standard validation.

Do not inherit directly from _Field_ class, because the implementation may change. Only SeLite Settings defines its subclasses. That's why there are two axis for validation - standard & custom. You can have a field with custom validation without having a special subclass for it.

# Customising existing fields #
Frameworks can add or remove fields from existing configuration modules (like _extensions.selite-settings.common_) and they can add keys to existing fields. See

  * [serendipity-framework.js](https://code.google.com/p/selite/source/browse/serendipity/serendipity-framework.js) and
  * [SettingsLogins](SettingsLogins) > [Adding custom keys](SettingsLogins#adding-custom-keys).