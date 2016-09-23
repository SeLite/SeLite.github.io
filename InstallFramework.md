---
layout: default
---
{% include links %}
* TOC
{:toc}

# Scope and downloads #
This assumes that you've set up your web application and that there is an SeLite framework for it. You'll need packages mentioned at [Overview](./) > [Install](./#install). If you haven't downloaded SeLite [Components](Components) yet, getting them from source (which you'll need anyway) may be faster.

Regardless of how you install [Components](Components), either

 * download a ZIP of a chosen framework as per [AppsFrameworks](AppsFrameworks). However, then you can't maintain it with GIT. Or
 * follow [InstallFromSource](InstallFromSource) > [Get the source](InstallFromSource#get-the-source). If you're installing components from source, follow [Install components from source](InstallFromSource#install-components-from-source).

If your web application uses SQLite, you'll get full SeLite functionality and smoother script data life cycle. You'll be able to copy/restore {{ appDB }}, {{ scriptDB }} and {{ vanillaDB}} from within SeLite (via [SettingsInterface](SettingsInterface)). Otherwise you need to apply [DataImport](DataImport).

The following instructions are generic. For any application-specific steps see documentation of the respective framework.

# Configuration #

## Areas, methods and stages of configuration ##
You need to configure two main areas

  * what framework (or any other bootstrapped extensions) to load
  * common and framework/extension-specific settings

There are two methods for this. The simpler one manages it through text file(s), as per {{navValuesManifests}}. Such configuration is easier to share and replicate than one administered through GUI. It also involves less steps and only one stage. You can still use GUI to review effect of {{valuesManifest}}s (though it involves some extra steps).

An alternative method is Firefox profile-based configuration set(s), controlled through GUI. It can also override any {{valuesManifest}}s. It's done through {{navAssociationsManifests}}. However, using GUI (for any part of configuration) usually requires the whole configuration to be in two stages.

## Configure loading the framework ##

### Through _values_ manifests ###
This is already done for [scripts][script] that come with SeLite frameworks. If you're using one of those frameworks, you can copy its {{ valuesManifest }} `SeLiteSettingsValues.txt`. Otherwise create it as a plain text file. Either way, you then need to adjust/enter value of field `extensions.selite-settings.common.bootstrappedCoreExtensions`, so it points to location of the framework Javascript file. See also {{navValuesManifests}} and {{navLiteralsForSpecialValues}}.

### Through GUI
See [SettingsInterface](SettingsInterface) > [Managing mode](SettingsInterface#managing-mode). Select 'Add a new value' next to `bootstrapCoreExtensions`. Point it to where you downloaded the framework JS file.

## Edit or review configuration of the framework ##

### Edit through _values_ manifests ###
Add values for applicable following fields and for any custom fields/keys to `SeLiteSettingsValues.txt` from the above

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
This has a dual purpose: to maintain profile-based configuration set(s), and to review effect of configuration set(s) or {{valuesManifest}}(s). If you want to use configuration set(s), create them if you haven't done so (see above).

Before you use GUI, you need to load the bootstrapped framework. Start Selenium IDE and run any single command, e.g. `getEval | true`. That has effect only for the current run of Firefox (so next time you start the browser you'll need to repeat it in order to use GUI). Then you can configure roles and any framework-specific fields.

If you'd like to review configuration, open {{chromeUrl}} _chrome://selite-settings/content/tree.xul?selectFolder_ (or _chrome://selite-settings/content/tree.xul?folder=/full/path/to/folder_).

If you'd like to edit profile-based configuration set(s), open _chrome://selite-settings/content/tree.xul_. Then follow [SettingsInterface](SettingsInterface) and edit the following fields (if they apply to your framework):

  * Enter usernames for roles that you need.
  * Point the configuration [set] at your app/script/vanilla SQLite files. You need at least {{ scriptDB }} to get benefits of [Overview](./) > [Data separation (in testing)](./#data-separation-in-testing). {{appDB}} and {{vanillaDB}} make sense only if the application data is in SQLite.
    * For {{appDB}} select the SQLite file which is used by your application instance. (The file often has an extension other than `.sqlite`, e.g. `.db` or even `.php`.)
    * {{scriptDB}} is for [scripts][script]. {{vanillaDB}} will serve as a snapshot of {{appDB}}, so that you can revert `appDB` and `testDB` to it. Enter some new filenames (in a location where your account can create files).
    * If you haven't got existing `testDB` and `vanillaDB`, in Selenium IDE click at button ![Reload Vanilla and Test](https://raw.githubusercontent.com/selite/selite/master/settings/src/chrome/skin/classic/reload_vanilla_and_test.png). That reloads vanilla DB and script DB from app DB. (See [SettingsInterface](SettingsInterface)).
  * Fill in `webRoot` (it doesn't matter whether it ends with a slash or not). Your [scripts][script] can access it via `SeLiteSettings.webRoot()`. (This is a workaround for Selenium IDE issue ['Base URL Should Allow Path'](https://github.com/SeleniumHQ/selenium/issues/1550). Please, vote for it and also for other [ThirdPartyIssues](ThirdPartyIssues).)
  * Fill in `tablePrefix`.
  * Open the URL of the installation, log in with account(s) that you entered for role(s) above and make Firefox save your password(s). (That's for [SettingsLogins](SettingsLogins).)
  * You may want to activate [Auto Check] to detect notices/warnings/errors. Currently that works out-of-the-box for PHP only.

# Run scripts
Unless you are using GUI to maintain or review configuration, you can run [scripts][script] ([cases][case] or [suites][suite]) right after you start Selenium IDE. You don't need to run any single Selenese command first.

Locate, open and run a [suite] as per [PackagedScripts](PackagedScripts).

## No hot switching between frameworks ##
If you have two or more frameworks, don't switch between them during the same Firefox run. You need to restart Firefox (not just Selenium IDE). Reasons:

  * Frameworks can remove configuration fields that are not relevant to them, or they add new ones. Such fields stay removed/added during the rest of Firefox run, even after you switch to a different framework.
  * If you start with one framework, switch to another one and then back to the first one (all within the same Firefox run), the first framework won't be re-applied unless its file was modified (as per [Bootstrap](Bootstrap) > [Switching between files](Bootstrap#switching-between-files)).
  * Frameworks usually benefit from `setTestDbKeeper()`, but only one framework (or extension) can invoke it. See {{navPreservingSpecialValuesInScriptDb}}.