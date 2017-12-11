---
layout: default
---
{% include links %}
* TOC
{:toc}

This is on top of [AddOnsThirdParty](AddOnsThirdParty).

# Running with web-ext#
Use firefox --ProfileManager to create a new profile. Then start web-ext --firefox-profile with that profile. Don't let web-ext create a new profile, as it will name it 'dev-edition-default'. If you rename it later (with firefox --ProfileManager), web-ext seems not able to pick it up!

See also [MDN: web-ext](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Getting_started_with_web-ext).

Most of the following is obsolete.

# Development and debugging #

This is for developing [Core extensions][core extension] and SeLite frameworks (as per [GeneralFramework](GeneralFramework)). Apply this in parallel to

  * [AddOnsThirdParty](AddOnsThirdParty)
  * [Setting up extension development environment (MDN)](https://developer.mozilla.org/en-US/Add-ons/Setting_up_extension_development_environment). Following are the relevant sections (in order of general importance):
    * see [Development profile](https://developer.mozilla.org/en-US/Add-ons/Setting_up_extension_development_environment#Development_profile) on how to run separate profiles of Firefox at the same time
    * skip 'Development preferences', see below instead
    * if you develop your own packaged extensions (without [Bootstrap](Bootstrap)), then read [Firefox extension proxy file](https://developer.mozilla.org/en-US/Add-ons/Setting_up_extension_development_environment#Firefox_extension_proxy_file). See also [setup_proxies.bat](https://github.com/selite/selite/blob/master/setup_proxies.bat) or [setup_proxies.sh](https://github.com/selite/selite/blob/master/setup_proxies.sh).
    * if you start Firefox from automated commandline/shell scripts, read 'Preventing the first launch extension selector'
  * [Debugging JavaScript (MDN)](https://developer.mozilla.org/en/docs/Debugging_JavaScript)
  * [Debugging Extensions (MDN)](https://developer.mozilla.org/en-US/docs/Building_an_Extension#Debugging_Extensions)
  * [TroubleShooting](TroubleShooting)

For the easiest download get all components of [SeLite Development Tools collection](https://addons.mozilla.org/en-GB/firefox/collections/peter-kehl/selite-development-tools/). Otherwise install

 * [AMO admin assistant](https://addons.mozilla.org/en-US/firefox/addon/amo-admin-assistant/)
 * [DOM Inspector](https://addons.mozilla.org/en-US/firefox/addon/dom-inspector-6622/)
 * [DevPrefs](https://addons.mozilla.org/en-US/firefox/addon/devprefs/). If you don't install DevPrefs, visit Firefox URL _about:config_ and set the following preferences:
  * `browser.tabs.remote.autostart` to boolean `false`. This turns electrolysis (e10s) until Selenium IDE works with it. (Alternatively, turn e10s at Firefox URL _about:preferences#general_ > 'Enable multi-process').
  * `xpinstall.signatures.required` to boolean `false`
  * `browser.dom.window.dump.enabled` to boolean `true`
  * `devtools.chrome.enabled` to boolean `true`
  * `devtools.debugger.remote-enabled` to boolean `true`
  * `devtools.debugger.prompt-connection` to boolean `false`. Otherwise, when you start Browser Toolbox, it shows up greyed out. Then you need to switch back to the main Firefox window, which show a modal dialog regarding the access that you need to allow. Disable this only if you are on a secured network.
  * add `nglayout.debug.disable_xul_cache` as boolean `true` (this preference is not set by default). It is for developing XUL GUI or its Javascript.

Regardless of whether you installed DevPrefs add-on or whether you set the above preferences manually, now set a few more preferences. Visit Firefox URL _about:config_ and

  * set `devtools.debugger.prompt-connection` to boolean `false`. Otherwise, when you start Browser Toolbox, it shows up greyed out. Then you need to switch back to the main Firefox window, which show a modal dialog regarding the access that you need to allow. Disable this only if you are on a secured network.
  * add `dom.allow_XUL_XBL_for_file` as boolean `true` (this preference is not set by default). It is for developing XUL GUI. (It enables access to `.xul` files via _file://_ URLs in addition to using {{chromeUrl}}s. Beware that such files are limited, with less access than `.xul` files under _chrome://_ URLs (e.g. no access to `Components.utils.import()`).

Restart Firefox, so that the above configuration takes effect.

## Browser Console and Toolbox ##
These are for development of SeLite frameworks, Selenium IDE and its extensions. For debugging your web application use Firefox menu Tools > Web Developer > Toggle Tools instead.

Find these in Firefox menu Tools > Web Developer > Browser Console (shortcut `Ctrl+Shift+J`) and Browser Toolbox (formerly Browser Debugger; since Firefox 39 it has a shortcut `Ctrl+Alt+Shift+I`). If you start `firefox` binary from a shell on Linux, messages from Browser Console also show up in that shell. You can also start `firefox.exe` or `firefox` binary with Browser Console and/or Browser Toolbox by passing parameters `-jsconsole` or `-jsdebugger` (as per [command line options](https://developer.mozilla.org/en-US/docs/Mozilla/Command_Line_Options)).

In Browser Console, JS tab/dropdown has level Warnings. Name of that level is confusing. Undefined variables in strict mode and some syntax errors generate messages at visible only Warnings level (or more detailed), but they halt execution of the script anyway. So if things don't add up, include `Warnings` level.

## Evaluating expressions for XUL##
Firefox menu Tools > Web Developer > Scratchpad (Shift+F4) doesn’t work for XUL files loaded via e.g. `chrome://extension/path/file-name.xul` (regardless of Environment setting - whether Content or Browser). Instead, use Firebug’s Console.

### Logging ###
Don't use `alert(message)` since it's disruptive. Also, it's not available in Javascript code modules.

In [Core scope] (i.e. in files listed for `coreUrl` in `SeLiteExtensionSequencer.js`) you can generate messages for Selenium IDE Log tab with

```javascript

LOG.debug(...);
LOG.info(...);
LOG.warn(...);
LOG.error(...);
```

Beware that even though there is `LOG` object also in Selenium IDE scope (i.e. in files listed for `ideURL` in `SeLiteExtensionSequencer.js`), it doesn't work (the messages don't show up in Selenium IDE log). Use `editor.getUserLog()` instead.

In Javascript code modules call `SeLiteMisc.log()`, which gives you Selenium Core `LOG` object. Alternatively, use `console` object, which logs to Firefox menu > Tools > Web Developer > Browser Console:

```javascript

var console= Components.utils.import("resource://gre/modules/Console.jsm", {}).console;
console.log(...);
console.info(...);
console.warn(...);
console.error(...);
```

### Browser Toolbox ###
  * it catches **some** events from Javascript code modules - ones loaded via `Components.utils.import()`
  * don't let it catch all exceptions - it reports too many annoyances from Firefox itself
  * it doesn't show stack trace of exceptions in its watch pane. So just add `expressionVariableName.stack` as a watched expression.
  * locate a Javascript file by its name in Sources list on the left, or by 'Search scripts' field at the top-right. However, they only list files that were used already, so some Selenium IDE files get loaded only after you run a Selenese command (e.g. `getEval | true`).
  * if you update a Javascript file (other than a code module) used by XUL and you enabled `nglayout.debug.disable_xul_cache`, then Firefox can refresh such Javascript, but browser debugger doesn't (as of Firefox 32).

Don't use [Javascript debugger](https://addons.mozilla.org/en-US/firefox/addon/javascript-debugger) (Venkman) (because as of Firefox 22 it couldn't locate Javascript code modules). Neither use [Tiny Javascript Debugger](http://sourceforge.net/u/pbrunschwig/tinyjsd/wiki/Home/) (as it doesn't catch `debugger` keyword).

#### Setting up breakpoints ####
If you can locate the source file in Firefox Browser Toolbox/Debugger, you can set up breakpoints by clicking at the left margin (at the line numbers).

#### `debugger` keyword ####
If the source file hasn't been loaded yet and you want to set the breakpoint beforehand, you need to modify the source. Follow [InstallFromSource](InstallFromSource) and set up proxy file(s). Then edit the source and add a line containing: `debugger;` When Firefox reaches such a line, it pauses there (but it doesn’t switch to Browser Toolbox automatically - you need to do that.)

Then (re)start Firefox, open Browser Toolbox/Debugger and only after that open Selenium IDE. The debugger will pause on that line.

You can also start Firefox with debugger from shell, i.e. `firefox -jsdebugger`. However, it seems that you can't use keyword `debugger` to investigate the very first loading stage of extensions (e.g. when [Extension Sequencer] loads an extension's `SeLiteExtensionSequencerManifest.js`). The debugger doesn't stop there. Instead, see above how to print messages with `console`.

#### Breakpoints in Core extensions ####
Selenium IDE loads custom [Core extensions][core extension] twice (reported in [ThirdPartyIssues](ThirdPartyIssues) > [Core extensions are loaded 2x](https://github.com/SeleniumHQ/selenium/issues/1549)). That applies regardless of whether you use Selenium IDE menu Options > Options > General > Selenium Core extensions, or whether you use [Extension Sequencer].

Custom Core file(s) get first loaded at the start of Selenium IDE. Then they get loaded once more when you run a Selenese command (or a whole [case] or [suite]) for the first time. So, when you use Firefox Browser Toolbox/Debugger, do it only after you've run at least one Selenese command. Browser Toolbox/Debugger reports those files with `?numericTimeStamp` appended to their file names. For each [Core extension] pick the one that has the highest timestamp.

This doesn't affect Javascript code modules (accessed via `Components.utils.import( 'chrome://...', optionalScope )`) - those get loaded once only. If you use [Bootstrap](Bootstrap) to load your extension(s) and you modify them while debugging, browser debugger will probably not refresh them.

#### Source of functions ####
If you inspect a variable that refers to a function (including a function field of an object/prototype), browser debugger doesn't list `toSource()` on the function object by default. You have to add it to Watch pane.

### Selenium IDE settings
When developing Selenium extensions, you'll most likely benefit from unchecking the following two options in Selenium IDE > Options > Options... > Plugins (as per [TroubleShooting](TroubleShooting) > [Loading Selenium plugins](TroubleShooting#loading_selenium_plugins)):

  * 'Disabling plugin should completely disable the addon in Firefox'
  * 'Automatically disable plugin provided code on plugin errors'. Disabling this speeds up the development process. However, if your plugin partially fails when it's loaded (i.e. because of a runtime error but not a syntax error), then Selenium doesn't report the problem! It will only list the error in in Selenium IDE menu > Options > Options... > Plugins > _pluginName_. That can be confusing. So if things don't add up, check the plugin there.

### Debugging and extending Selenium IDE GUI
Say you'd like to debug or extend Selenium IDE GUI or Selenium IDE add-ons (rather than Selenium Core add-ons). If you want to match the GUI parts to its source, open {{chromeUrl}} _chrome://selenium-ide/content/selenium-ide.xul_ and inspect it with Firebug. It's not as easy as inspecting an HTML page. If you use the inspect pointer button, that won't expand down to all elements. Then you may need to open HTML tab and navigate in its breadcrumb, or alternate between using the inspector pointer and HTML tab. Then search in the relevant source for XML elements that you've identified.

## Identifying event handlers ##
When figuring out event handlers in your web application you can use

  * [Visual Event](http://www.sprymedia.co.uk/article/Visual+Event) bookmarklet
  * [EventBug](http://www.softwareishard.com/blog/firebug/eventbug-alpha-released/) for FireBug
  * [JQuery Debugger](https://chrome.google.com/webstore/detail/jquery-debugger/dbhhnnnpaeobfddmlalhnehgclcmjimi) for Chrome

# NetBeans as a Javascript IDE #
[NetBeans](https://netbeans.org/downloads/) is a good IDE for several languages, including Javascript. Essential features are

  * syntax highlighting (and validation)
  * highlighting of undefined variables
  * coding assistance (where it can determine it)
    * code completion
    * Ctrl+click at a function/object/variable name navigates to its definition
    * Ctrl+mouse over a function/object shows its documentation in a popup

## Version ##
Get NetBeans 8.0.1+ for HTML5 & PHP and

  * don't use HTML5 projects - then navigation to Javascript code modules didn't work (as of version 7.4)
  * create a PHP project, with your Javascript files anywhere under the source folder
  * download SeLite sources or check them out from git, either
    * into a subfolder within your project, or
    * into a separate folder (outside your project); then
      * in project context menu > Properties > 'PHP include path' choose the folder where you have checked out SeLite
        * you can add multiple folders, if you need to include other sources, too
      * do not use project context menu > Properties > 'JavaScript Files' > 'Libraries Folder' to load the JS sources, because then source navigation didn't work

## Plugins ##
If you also want to write custom SQLite filters/importers, you can add Java SE support to your NetBeans via menu Tools > Plugins. Then you'll also need Java SE JDK.

Don't install [Foxbeans](http://www.teesoft.info/content/view/36/36/#Foxbeans), a plugin to develop Mozilla addons in NetBeans. Otherwise, `install.rdf` files wouldn’t save on Ctrl+S (even when using a PHP Project, not an Addons project).

## Custom code folding ##
If you repeat a pattern of code or similar sections of code, consider applying [How to quickly create editor fold](http://stackoverflow.com/questions/11251770/how-to-quickly-create-editor-fold) (which uses [SurroundWithCodeFolding](http://wiki.netbeans.org/SurroundWithCodeFolding)).

# Hosting add-ons at addons.mozilla.org #
If you host your extensions at [addons.mozilla.org](https://addons.mozilla.org/):

* See [Components](Components) > [Latest releases](Components#latest-releases).
* When uploading a new version, do not remove all previously reviewed versions, but keep at least one. Otherwise _https://addons.mozilla.org/en-US/firefox/addon/XXXXX/_ and _https://addons.mozilla.org/en-US/firefox/addon/XXXXX/versions/_ will stop working. (However, _https://addons.mozilla.org/en-US/developers/addon/XXXXX/edit_ will still work and [Manage My Submissions](https://addons.mozilla.org/en-US/developers/addons) will still list the extension.) Vote for fixing this at [ThirdPartyIssues](ThirdPartyIssues) > Show version awaiting review in detail page....
* If you create a new add-on, upload it to _addons.mozilla.org_ early. They sometimes delayed the initial review by over a month. Until then your add-on is not public (not even at _https://addons.mozilla.org/en-US/firefox/addon/XXXXX/versions/_)!
* See [Mozilla bug on selecting dependencies](https://bugzilla.mozilla.org/show_bug.cgi?id=1239191).