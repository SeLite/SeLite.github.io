---
title: Settings Manifests
layout: default
---


# Purpose of manifests #
Manifests connect test suites to configuration sets, or they define configuration values. They let you define

  * default configuration (or its parts) specific to a folder of test suite(s)
  * associations between tests and profile-based configuration sets
  * overrides at various levels.

Manifests are optional. A configuration module (schema) can opt out of using them. Such a module then only uses module default values, and the default set (if any and if the it allows multiple sets) or the only one set (if it doesn't allow multiple sets).

There are two types of manifests: containing _values_ or _associations_. They both apply to any test suites under their folder tree. See [SettingsScope](SettingsScope).

Manifests are plain text files, so you can share and manage them through source control. Empty lines are skipped, and so are lines starting with a hash # (which serve for comments).

# _Values_ manifests #
These are in files called in `SeLiteSettingsValues.txt` . They define configuration values (in addition to profile-based configurations sets managed through [SettingsInterface](SettingsInterface)). Values are entered in simple format, one per line:

```
moduleName.fieldName value
moduleName.fixedMapFieldName:key value
```

`moduleName` is the name of your custom configuration module (for common frameworks it's usually `extensions.selite-settings.common`). Module names, field names and FixedMap keys can't contain any whitespace, so this syntax is non-ambiguous. Values can contain any characters (except for line breaks).

If a field allows multiple values, you can list them on per line; they will be loaded in that order. However, they have to be listed in the same manifest file (see [SettingsScope](SettingsScope) > [Multi-valued fields](SettingsScope#multi-valued-fields)) - they won't merge from different scopes.

## Literals for special values ##
For single-valued fields you can use literal `SELITE_NULL` to represent Javascript's null. For multi-valued fields there you can use literal `SELITE_VALUE_PRESENT` to represent an empty list/choice.

For File fields, you can start their value(s) with literal `SELITE_THIS_MANIFEST_FOLDER`. That gets replaced with the full path to the manifest's folder. Also, such values (relative to the manifest's folder) can contain a  folder separator (\ or / - either will work on any system).

# _Associations_ manifests #
These are in files called `SeLiteSettingsAssociations.txt`. They connect tests and profile-based configuration sets. Associations are entered in simple format, one per line. Module names and set names can't contain a space, so this syntax is non-ambiguous:

```
moduleName setName
```

A configuration set can be linked to any number of folders. A folder can have maximum of one set associated with it per module at its own level. However, it also inherits any associations from higher folder(s) - see [SettingsScope](SettingsScope).
