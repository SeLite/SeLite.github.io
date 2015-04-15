---
title: SeLite development tools
layout: default
---

# Development and debugging #

Read this in parallel to
  * [Setting up extension development environment (MDN)](https://developer.mozilla.org/en-US/Add-ons/Setting_up_extension_development_environment). Following are the relevant sections (in order of general importance):
    * see [Development profile](https://developer.mozilla.org/en-US/Add-ons/Setting_up_extension_development_environment#Development_profile) on how to run separate profiles of Firefox at the same time
    * skip 'Development preferences', see below instead
    * if you develop your own packaged extensions (without [BootstrapLoader](BootstrapLoader)), then read [Firefox extension proxy file](https://developer.mozilla.org/en-US/Add-ons/Setting_up_extension_development_environment#Firefox_extension_proxy_file). See also <a href='https://code.google.com/p/selite/source/browse/setup_proxies.bat'>setup_proxies.bat</a> or <a href='https://code.google.com/p/selite/source/browse/setup_proxies.sh'>setup_proxies.sh</a>.
    * if you start Firefox from automated scripts, read 'Preventing the first launch extension selector'
  * [Debugging JavaScript (MDN)](https://developer.mozilla.org/en/docs/Debugging_JavaScript)
  * [Debugging Extensions (MDN)](https://developer.mozilla.org/en-US/docs/Building_an_Extension#Debugging_Extensions)
  * [TroubleShooting](TroubleShooting)

Visit Firefox URL _about:config_ and set the following preferences:
  * _browser.dom.window.dump.enabled_ to boolean true
  * _devtools.chrome.enabled to boolean_ true
  * _devtools.debugger.remote-enabled_ to boolean true
  * _devtools.debugger.prompt-connection_ to boolean false: Do so only if you are on a secured network.
  * Following are for developing XUL GUI or its javascript. These preferences don't exist by default
    * _nglayout.debug.disable\_xul\_cache_ to boolean true
    * _dom.allow\_XUL\_XBL\_for\_file_ to boolean true: For accessing .xul files via _file://_ URLs (in addition to _chrome://_). Beware that such files are limited, with less access than .xul files under _chrome://_ URLs (e.g. no access to _Components.utils.import_()).

Install plugins listed at [AddOnsThirdParty](AddOnsThirdParty). Restart Firefox, so that the above configuration takes effect.

## Browser Console and Toolbox ##
You have these in Firefox menu Tools > Web Developer > Browser Console and Browser Toolbox (formerly Browser Debugger; since Firefox 39 it has a shortcut <i>Ctrl+Alt+Shift+I</i>). If you start _firefox_ binary from a shell on Linux, messages from Browser Console also show up in that shell. You can also start _firefox.exe_ or _firefox_ binary with Browser Console and/or Browser Toolbox by passing parameters _-jsconsole_ or _-jsdebugger_ (as per [command line options](https://developer.mozilla.org/en-US/docs/Mozilla/Command_Line_Options)).

In Browser Console, 'JS' tab/dropdown has level 'Warnings'. Name of that level is confusing. Some syntax errors or undefined variables in strict mode only generate messages at 'Warnings' level, but they halt execution of the script. So if things don't add up, include that level.

### Logging ###
Don't use _alert(message)_ since it's disruptive. Also, it's not available in Javascript code modules.

In Selenium Core scope (i.e. in files listed for 'coreUrl' in _SeLiteExtensionSequencer.js_) you can generate messages for Selenium IDE 'Log' with
```javascript

LOG.debug(...);
LOG.info(...);
LOG.warn(...);
LOG.error(...);
```
Beware that while there is 'LOG' object also in Selenium IDE scope (i.e. in files listed for 'ideUrl' in _SeLiteExtensionSequencer.js_), it doesn't work (the messages don't show up in Selenium IDE log). Use _editor.getUserLog()_ instead.

In Javascript code modules use _console_ object, which logs to Firefox menu > Tools > Web Developer > Browser Console:
```javascript

var console= Components.utils.import("resource://gre/modules/devtools/Console.jsm", {}).console;
console.log(...);
console.info(...);
console.warn(...);
console.error(...);
```

### Browser Toolbox ###
If you don't disable _devtools.debugger.prompt-connection_ and you start Browser Toolbox, it shows up greyed out. Then you need to switch back to the main Firefox window, which show a modal dialog regarding the access that you need to allow.
  * it catches **some** events from Javascript code modules - ones loaded via Components.utils.import()
  * don't let it catch all exceptions - it reports too many annoyances from Firefox itself
  * it doesn't show stack trace of exceptions in its watch pane. So just add 'e.stack' (or expressionVariableName.stack) as a watched expression.
  * you can locate a javascript file by its name in 'Sources' list on the left, or 'Search scripts' field at the top-right. However, they only list files that were used already, so some Selenium IDE files get loaded only after you run a Selenese command (e.g. `getEval|true`).
  * if you update a javascript file (other than a code module) used by XUL and you enabled nglayout.debug.disable\_xul\_cache, then Firefox can refresh such javascript, but browser debugger doesn't (as of Firefox 32).

Don't use [Javascript debugger](https://addons.mozilla.org/en-US/firefox/addon/javascript-debugger) (Venkman) (because as of Firefox 22 it couldn't locate Javascript code modules).

#### Setting up breakpoints ####
If you can locate the source file in Firefox Browser Toolbox/Debugger, you can set up breakpoints by clicking at the left margin (at the line numbers).

#### _debugger_ keyword ####
If the source file hasn't been loaded yet and you want to set the breakpoint beforehand, you need to modify the source. Follow [InstallFromSource](InstallFromSource) and set up proxy file(s). Then edit the source and add a line containing: _debugger;_ (When Firefox reaches such a line, it pauses there but it doesn’t switch to Browser Toolbox automatically - you need to do that.)

Then (re)start Firefox, open Browser Toolbox/Debugger and only after that open Selenium IDE. The debugger will pause on that line. However, that sometimes didn't get triggerred.

You can also start Firefox with debugger from shell, i.e. <i>firefox -jsdebugger</i>. However, it seems that you can't use keyword _debugger_ to investigate the very first loading stage of extensions (e.g. when [ExtensionSequencer](ExtensionSequencer) loads an extension's SeLiteExtensionSequencerManifest.js). The debugger doesn't stop there. You'd need to print messages with    _console_.

#### Breakpoints in Core extensions ####
Selenium IDE loads some Core parts twice (reported in [ThirdPartyIssues](ThirdPartyIssues) > [Selenium IDE issue #6697](http://code.google.com/p/selenium/issues/detail?id=6697)). That applies if you use Selenium IDE menu Options > Options > General > Selenium Core extensions, or if you use [ExtensionSequencer](ExtensionSequencer) (which also loads any SeLite extensions). This doesn't affect Javascript code modules (loaded via _Components.utils.import( 'chrome://...', optionalScope )_) - those get loaded once only.

The file(s) get first loaded at the start of Selenium IDE. Then they get loaded once more when you run a Selenese command (or a whole test case or test suite) for the first time.

When you use Firefox Browser Toolbox/Debugger, do it only after you've run at least one Selenese command. Browser Toolbox/Debugger reports those files with _?numericTimeStamp_ appended to their file names. For each Core extension pick the one that has the highest timestamp.

#### Debugging and inspecting Selenium IDE GUI ####
Say you'd like to debug or extend Selenium IDE GUI. If you want to match the GUI parts to its source, open <i>chrome://selenium-ide/content/selenium-ide.xul</i> and inspect it with Firebug. It's not as easy as inspecting an HTML page. If you use the inspect pointer button, that won't expand down to all elements. Then you may need to open HTML tab and navigate in its breadcrumb, or alternate between using the inspector pointer and HTML tab.

#### Source of functions ####
If you inspect a variable that refers to a function, or you inspect a function field of an object/prototype, Browser Toolbox/Debugger doesn't list toSource() on the function object itself by default. You have to add it to Watch pane. But you shouldn't need this if you apply [JavascriptEssential](JavascriptEssential) > [Avoid nameless functions](JavascriptEssential#avoid-nameless-functions).

## Identifying event handlers ##
When figuring out event handlers in your web application you can use
  * [Visual Event](http://www.sprymedia.co.uk/article/Visual+Event) bookmarklet
  * [EventBug](http://www.softwareishard.com/blog/firebug/eventbug-alpha-released/) for FireBug
  * [JQuery Debugger](https://chrome.google.com/webstore/detail/jquery-debugger/dbhhnnnpaeobfddmlalhnehgclcmjimi) for Chrome

## Selenium IDE settings ##
When developing Selenium extensions, you'll most likely benefit from unchecking the following two options in Selenium IDE > Options > Options... > Plugins (as per [TroubleShooting](TroubleShooting) > [Loading Selenium plugins](TroubleShooting#loading_selenium_plugins)):
  * <i>Automatically disable plugin provided code on plugin errors</i>. This speeds up the development process. However, if your plugin partially fails when it's loaded (i.e. because of a runtime error but not a syntax error), then Selenium doesn't report the problem! That can be confusing. So if things don't add up, check the plugin in Selenium IDE > Options > Options... > Plugins.
  * <i>Disabling plugin should completely disable the addon in Firefox</i>

# NetBeans as a Javascript IDE #
[NetBeans](https://netbeans.org/downloads/) is a good IDE for several languages, including Javascript. Essential features are
  * syntax highlighting (and validation)
  * highlighing of undefined variables
  * coding assistance (where it can determine it)
    * code completion
    * code navigation to the definition of functions/objects/variables by Ctrl+click at their name
    * documentation popup when you Ctrl+hover at a function or an object

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

There exists [Foxbeans](http://www.teesoft.info/content/view/36/36/#Foxbeans), a plugin to develop Mozilla addons in NetBeans. Beware that when installed, _install.rdf_ files wouldn’t save on Ctrl+S (even when using a PHP Project, not an Addons project).

## Custom code folding ##
If you repeat a pattern of code or similar sections of code, consider applying [How to quickly create editor fold](http://stackoverflow.com/questions/11251770/how-to-quickly-create-editor-fold) (which uses [SurroundWithCodeFolding](http://wiki.netbeans.org/SurroundWithCodeFolding)).

# Hosting add-ons at addons.mozilla.org #
If you host your extensions at [addons.mozilla.org](https://addons.mozilla.org/), beware of the following.

When you upload a new version of your add-on XXXXX, it won't show up on the main page of your add-on _https://addons.mozilla.org/en-US/firefox/addon/XXXXX/_ until Mozilla reviews it, which can take from a few days up to a month... The new version will be at _https://addons.mozilla.org/en-US/firefox/addon/XXXXX/versions/_ until then.

Do not remove all previously reviewed versions and keep at least the last one. Otherwise _https://addons.mozilla.org/en-US/firefox/addon/XXXXX/_ and _https://addons.mozilla.org/en-US/firefox/addon/XXXXX/versions/_ stop working, even though _https://addons.mozilla.org/en-US/developers/addon/XXXXX/edit_ still works and 'Manage My Submissions' (https://addons.mozilla.org/en-US/developers/addons) still lists the extension. Vote for fixing this at [ThirdPartyIssues](ThirdPartyIssues) > Show version awaiting review in detail page....