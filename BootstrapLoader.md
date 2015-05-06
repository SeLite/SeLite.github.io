---
title: Automated reloading of custom Selenium Core extensions
layout: default
---

# Summary #
[SeLite Bootstrap](https://addons.mozilla.org/en-US/firefox/addon/SeLite-Bootstrap/versions/), one of SeLite [AddOns](AddOns), allows smoother development of Selenium Core extensions (just plain javascript files, not .xpi). It reloads them automatically on change.

It's more convenient than Selenium IDE's way of loading Core extensions (Selenium menu > Options > Options > General > 'Activate developer tools'), which requires you to apply Selenium menu > Options > Options > General > 'Reload' button everytime you change your extension file.

# Details #
It adds field _bootstrapCoreExtensions_ to [Settings](Settings) module _extensions.selite-settings.common_, where you can choose file(s) containing your Core extension in Javascript (saved in UTF-8). If you're sharing your tests, you may want to configure that through [SettingsManifests](SettingsManifests) > [Literals for special values](SettingsManifests#literals-for-special-values).

If you modify any one file that is registered with Bootstrap (while using Selenium IDE), all the registered files get re-loaded automatically next time you

  * run a single Selenese command (before that command)
  * run a test case/test suite (before the case/suite)
  * pause and subsequently resume a test case/test suite (before it is resumed)

Apply [JavascriptEssential](JavascriptEssential), especially [Strict Javascript](JavascriptEssential#strict-javascript) and [Prevent name conflicts](JavascriptEssential#prevent-name-conflicts).

It works only in standalone Selenium IDE, but not in [auxiliary Selenium IDEs inside browser](SeleniumIDE#auxiliary-selenium-ides-inside_browser) (neither in Selenium IDE in browser sidebar).

# Limitations #

## Intercepts ##
Bootstrap reloads all registered extensions (whenever you modify any one of them). So, if you 'extend' an existing method as per [JavascriptEssential](JavascriptEssential) > [Function intercepts](JavascriptEssential#function-intercepts), then don't re-extend the __current__ method each time the script is run. Otherwise it would increase the intercept chain. Instead, save the original method on the first load only, and then re-extend it on every load. For example:

```js
var originalMethod;
if( originalMethod===undefined ) {
originalMethod= Selenium.prototype.methodName;
}
Selenium.prototype.methodName= function(pqr...) {
originalMethod.call(this);
// extra new tasks...
}
```
See also [JavascriptEssential](JavascriptEssential) > [Function intercepts](JavascriptEssential#function-intercepts).

## Adding/modifying the Selenese (commands/getters etc.) ##
If you introduce or modify any Selenese command/getter - _Selenium.prototype.doXXX, Selenium.prototype.getXXX_ or _Selenium.prototype.isXXX_, then those will be available to your test commands/case/test suite only **after** you run any Selenese command first. <!-- TODO don't know why -->

## Switching between files ##
If you remove a filename from _bootstrapCoreExtensions_ (or you switch to a different default set or a test suite with a different set associated with it), Bootstrap can't 'unload' a file that it has loaded already. If you change that option later and you add the filename back, it won't re-run the file, unless its timestamp has changed.

## Dependencies between files ##
Bootstrap initiates extensions after any Selenium Core extensions loaded as Firefox add-ons (whether they use [ExtensionSequencer](ExtensionSequencer) or not).

If you have multiple files registered with Bootstrap through a 'values' manifest (as per [SettingsManifests](SettingsManifests)), they get loaded in that order. However, if you register multiple files through profile-based configuration (as per [SettingsInterface](SettingsInterface)), their order is not guaranteed. Then you need more structure for that: package them as Firefox extensions and load them through [ExtensionSequencer](ExtensionSequencer).

On change, Bootstrap re-loads all registered files, not just the one(s) updated. That allows dependant files (that were registered with Bootstrap through a 'values' manifest) to inject any hooks in the expected order.