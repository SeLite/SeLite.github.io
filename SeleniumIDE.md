---
title: Selenium IDE notes/tips
layout: default
---

# Summary and scope
These notes about [Selenium IDE](http://seleniumhq.org/projects/ide) are on top of its [standard documentation](http://docs.seleniumhq.org/docs/02_selenium_ide.jsp). This assumes that you have installed all SeLite [AddOns](AddOns). If you develop test scripts, frameworks or plugins, see also [DevelopmentTools](DevelopmentTools).

# Auto-generated Selenese commands #
Selenese commands are defined in these primary forms: <i>xyz, <b>get</b>Xyz, <b>is</b>Xyz</i> and <i><b>is</b>Xyz<b>Present</b></i>. Selenium auto-generates their variations (listed below).

Selenium IDE shows the original reference for both the primary and auto-generated actions. However, the online reference contains only the primary actions/functions. So if you'd like to locate them online (or in the source), use the following. (See also <i>loadSeleniumCommands()</i> in Selenium IDE's treeView.js.)

<table>
<thead>
<tr>
    <td><b>Primary commands</b><br/> (as listed both online and in Reference tab)</td>
    <td><b>Auto-generated commands</b><br/>(listed in Reference tab only, but not online)</td>
</tr>
</thead>
<tbody>
<tr>
    <td> xyz<br>(implemented by function <b>do</b>Xyz)</td>
    <td> xyz<b>AndWait</b>                       </td>
</tr>
<tr>
    <td><b>assert</b>Xyz</td>
    <td><b>verify</b>Xyz</td>
</tr>
<tr>
    <td><b>get</b>Xyz<br><b>is</b>Xyz (other than <b>is</b>Xyz<b>Present</b>)<br> (rarely useful on their own,<br> use generated commands instead)</td>
    <td> <b>assert</b>Xyz<br><b>assertNot</b>Xyz</td>
</tr>
<tr>
    <td>&#160;</td>
    <td> <b>verify</b>Xyz<br><b>verifyNot</b>Xyz</td>
</tr>
<tr>
    <td>&#160;</td>
    <td> <b>store</b>Xyz</td>
</tr>
<tr>
    <td>&#160;</td>
    <td> <b>waitFor</b>Xyz<br><b>waitForNot</b>Xyz</td>
</tr>
<tr>
    <td> <b>is</b>Xyz<b>Present</b><br>(rarely useful on its own,<br> use generated commands instead) </td>
    <td> <b>assert</b>Xyz<b>Present</b> <br><b>assert</b>Xyz<b>NotPresent</b> </td>
</tr>
<tr>
    <td>&#160;</td>
    <td> <b>verify</b>Xyz<b>Present</b> <br><b>verify</b>Xyz<b>NotPresent</b> </td>
</tr>
<tr>
    <td>&#160;</td>
    <td> <b>store</b>Xyz<b>Present</b> </td>
</tr>
<tr>
    <td>&#160;</td>
    <td> <b>waitFor</b>Xyz<b>Present</b> <br><b>waitFor</b>Xyz<b>NotPresent</b>  </td>
</tr>
</tbody></table>

# Selenese parameters (Target and Value)
Selenese commands accept up to two parameters: Target and Value.
* Target is often a locator (an expression that identifies an element). The whole target can also be a Javascript expression (for _getEval_).
* One or multiple parts of Target or Value can be Javascript expressions, each enclosed within back ticks \`...\`. A few variations of back tick notation exist. See [EnhancedSyntax](EnhancedSyntax).
* If Target is a locator, Value is usually an expected or new value, which is compared or entered into element identified by Target. Value can also be a name of a stored variable, or something else.
* [Selenium Core reference](http://release.seleniumhq.org/selenium-core/1.0.1/reference.html) > [Element Locators](http://release.seleniumhq.org/selenium-core/1.0.1/reference.html#locators) is handy.

## XPath
Full XPath locators start with _xpath=_ or with //. Usually an XPath expression starts with //, with which you don't need to use _xpath=_. However, some SeLite commands (like _clickRandom_ and _selectRandom_) only accept XPath as the locator, but they require it not to contain any leading _xpath=_ prefix (whether the XPath starts with // or not).

See resources on XPath:
* [MDN](https://developer.mozilla.org/en-US/docs/Web/XPath)
* [W3C](http://www.w3.org/TR/xpath/)
* [XPath functions supported by Firefox](https://developer.mozilla.org/en-US/docs/XPath/Functions)
* [XPath siblings](http://stackoverflow.com/questions/365750/xpath-sibling-conditional-testing)

## UI-Element mappings
[UI Mapping](http://www.seleniumhq.org/docs/06_test_design_considerations.jsp#ui-mapping) (or 'UI-Element mapping') defines a mapping between meaningful names of elements on webpages, and the elements themselves. Element locators are in forms
* _ui=semanticPageName::semanticElementName(...)_ or
* _ui=semanticPageName::semanticElementName(...)->xpathOffsetLocator_.

(The example there is for Java only, while SeLite frameworks define UI Mappings in Javascript.) See also [detailed reference](https://selenium.googlecode.com/git/javascript/selenium-core/scripts/ui-doc.html). If you've installed Selenium IDE, you can get the same reference offline through Selenium IDE menu > Help > UI-Element Documentation or at a Firefox URL: _chrome://selenium-ide/content/selenium-core/scripts/ui-doc.html_.

# Variables
## Stored variables
Some Selenese actions store variables, to be used by further actions. [SelBlocksGlobal](SelBlocksGlobal) manages scope of such variables. When inside a [SelBlocksGlobal](SelBlocksGlobal) function, you can only use stored variables set in that function (or passed as parameters to it). The local scope also means: if you set a stored variable within a function and the same stored variable exists in the caller scope (that invoked the current function), the variable in the caller scope won't be affected.

Parameters of Selenese actions can access stored variables as `${name-of-the-variable}`. Those get replaced by the value of the variable. However, if the action processes the parameter as a Javascript expression (e.g. _storeEval_, _getEval_ or when using [EnhancedSyntax](EnhancedSyntax)), and if the variable contains an array/object or a non-numeric string (possibly with an apostrophe or quotation mark), then the replacement won't work robustly. For those cases use _storedVars.name-of-the-variable_ or <i>storedVars['name-of-the-variable']</i>. See also [EnhancedSyntax](EnhancedSyntax).

##Javascript variables
Sometimes you want a 'global' variable that spreads across the functions (which stored variables can't). Use 'direct' Javascript variables for it. Set them using command/action _getEval_ with the target being: <i>variable1=valueOrExpression, variable2=valueOrExpression....</i>

Don't use command _storeEval_ for that - it sets a stored variable, which is local.

# Limitations of getEval, storeEval
Command _getEval_ (and derived commands like _storeEval_ - as per [Auto-generated Selenese commands](#auto-generated-selenese-commands) above) etc. don’t like new line string literals _"\n"_ or _'\n'_ (or any string literals that contain them). Then they generate a confusing error 'unterminated string literal'. Use _String.fromCharCode(10)_ instead.

# Tips on GUI usability
When saving a test case or a test suite, Selenium IDE doesn't add _'.html'_ extension to the filename. So, add _.html_ yourself, so that later you can identify the file more easily.

## Using multiple Selenium IDEs in parallel
A running Firefox instance can show only one standard Selenium IDE window. Yet, viewing/editing different test cases in multiple Selenium IDE windows (at the same time) increases productivity. Several ways exist for it, varying in intuitiveness, simplicity and accessibility. Some involve multiple running instances of Firefox, with separate profiles.

Don't modify same test case or test suite in various Selenium IDE windows simultaneously.

### Auxiliary Selenium IDEs inside browser
This shows one (standard) Selenium IDE detached from Firefox browser windows. Other Selenium IDEs are inside browser windows, but they may look standard, too. Compared to the next method, this
* (+) is simpler to set up, to start and to maintain
* (+) involves less maintenance: it runs only one Firefox instance (with one profile)
* (+) is more convenient: shared bookmarks, window history, add-ons etc.
* (+-) shares history of recent files in Selenium IDE; however, it gets overwritten by the instance closed as the last one
* (-) can't run scripts in auxiliary Selenium IDEs (they target their own window/tab). If you modify a script there without saving it and your run a command that changes the current page, your changes will be lost! So you should use auxiliary Selenium IDEs primarily to view scripts (for reference). Auxiliary Selenium IDEs don't load any [SettingsManifests](SettingsManifests) neither any [SettingsOverview](SettingsOverview) sets associated with test suites. (That prevents potential conflicts due to the fact that any auxiliary Selenium IDEs share Selenium Core scope along with the standalone Selenium IDE, if any). Therefore they don't load any extensions through [BootstrapLoader](BootstrapLoader), either.

Steps

1. Install [Stylish](https://addons.mozilla.org/en-US/firefox/addon/stylish)
2. Install [Hide Tab Bar With One Tab](https://addons.mozilla.org/en-US/firefox/addon/hide-tab-bar-with-one-tab/)
3. Install [Hide Navigation Bar](https://addons.mozilla.org/en-us/firefox/addon/hide-navigation-bar/) (for usage see below)
4. Install [per-tab coloured menu text](https://userstyles.org/styles/110010/selenium-ide-per-tab-coloured-menu-text):
   <a href="https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshots/110010_after.jpeg?r=1424730593"><img src="https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshot_thumbnails/110010_after.jpeg?r=1424730593&extensionForGoogleCode=file.jpeg"/></a>
5. Restart Firefox
6. Start standalone Selenium IDE (by Ctrl+Alt+S, or Firefox menu Tools > Selenium IDE)
7. Display auxiliary Selenium IDEs. For each
 1. open a new Firefox window
 2. open one of URLs
    * _chrome://selenium-ide/content/selenium-ide.xul#GREEN_
    * _chrome://selenium-ide/content/selenium-ide.xul#BLUE_
    * _chrome://selenium-ide/content/selenium-ide.xul#RED_
    * _chrome://selenium-ide/content/selenium-ide.xul#WHITE_
 3. if you have already opened <i>chrome://selenium-ide/content/selenium-ide.xul</i> and later you add or change the hash part (<i>#GREEN, #BLUE</i>, <i>#RED</i> or <i>WHITE</i>), it won't take effect (even after you hit Enter) until you refresh the URL e.g. by F5 key (which will lose any modifications)
 4. hide Firefox navigation bar by pressing F2 (you may need to press it twice)
  * (+) it applies to the current window and any new windows later, but not to other existing Firefox windows
  * (-) however, after a Firefox restart it applies to all windows; then press F2 to show navigation bar where you want it

In auxiliary IDEs
* optionally, press F11 for full screen mode (this is probably beneficial only if you have more than two physical screens)
* (-) if you hide Firefox menu, it often doesn't show up when you press Alt key. Then you need to click at the very top of the browser window (which shows <i>testCaseName (testCaseFileName.html) - Selenium IDE X.Y.Z - Mozilla Firefox</i>) and then press Alt.
* (-) pressing F5 (which reloads Selenium IDE), or closing Selenium IDE tab/window with 'x' icon, applies without any confirmation about unsaved test cases/test suite.

There is also Firefox menu > View > Sidebar > Selenium IDE. However, that Selenium IDE sidebar has restricted width. Like Auxiliary Selenium IDEs, Selenium IDE in a sidebar doesn't load any [SettingsManifests](SettingsManifests) neither any [SettingsOverview](SettingsOverview) sets. Please, vote at [ThirdPartyIssues](ThirdPartyIssues) > [Sidebars (history, bookmarks) should not have maximum width](https://bugzilla.mozilla.org/show_bug.cgi?id=406629) so that it gets fixed.

### Multiple standard Selenium IDEs
This shows multiple Selenium IDEs, detached from Firefox browser windows. Compared to the previous method, this
* (-) needs multiple running Firefox instances (and profiles). Each profile each needs an installation of Selenium IDE; ones that run scripts also need their own installations of SeLite [AddOns](AddOns) and any other extensions.
* (+) can run multiple scripts in parallel with separate sessions (cookies)

To set up

1. Create Firefox profiles, one per Selenium IDE: Follow [Setting up extension development environment (MDN)](https://developer.mozilla.org/en-US/Add-ons/Setting_up_extension_development_environment) > [Development profile](https://developer.mozilla.org/en-US/Add-ons/Setting_up_extension_development_environment#Development_profile).
2. Start multiple Firefox instances, one per profile, with command line parameters <i>-no-remote</i> and (<i>-P ProfileName</i>).
3. Install Selenium IDE in each profile. Install SeLite [AddOns](AddOns) any other extensions in profiles where you will run scripts.
4. Mark the second and successive profiles to identify Selenium IDEs:
  * Visually by colour
   1. Install [Stylish](https://addons.mozilla.org/en-US/firefox/addon/stylish)
   2. Restart Firefox
   3. Install styles for the second and further Selenium IDEs:
     * [green menu text](https://userstyles.org/styles/109886/selenium-ide-green-menu-text):
       <a href='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshots/109886_after.jpeg?r=1422833554'><img src='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshot_thumbnails/109886_after.jpeg?r=1422833554&extensionForGoogleCode=file.jpeg' /></a>
     * <a href='https://userstyles.org/styles/110005/selenium-ide-blue-menu-text'>blue menu text</a>:<br> <a href='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshots/110005_after.jpeg?r=1422833463'><img src='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshot_thumbnails/110005_after.jpeg?r=1422833463&extensionForGoogleCode=file.jpeg' /></a>
     * <a href='https://userstyles.org/styles/110006/selenium-ide-red-menu-text'>red menu text</a>:<br> <a href='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshots/110620_after.jpeg?r=1424729821'><img src='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshot_thumbnails/110620_after.jpeg?r=1424729821&extensionForGoogleCode=file.jpeg' /></a>
     * <a href='https://userstyles.org/styles/110620/selenium-ide-white-menu-text'>white menu text</a>:<br> <a href='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshots/110620_after.jpeg?r=1424728011'><img src='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshot_thumbnails/110620_after.jpeg?r=1424728011&extensionForGoogleCode=file.jpeg' /></a>
     * You can switch between styles in Firefox menu Tools > Add-ons > User Styles.
  * By language (this applies to the whole Firefox instance, rather than just Selenium IDE, which can make it difficult to use!)
    1. Install [Simple Locale Switcher](https://addons.mozilla.org/en-US/firefox/addon/simple-locale-switcher/)
    2. Restart Firefox
    3. Install different language packs through Firefox toolbar > 'Simple Locale Switcher' green button > 'Get more language packs...'

#### Custom styles for Stylish
Use can create or adjust a style in Firefox menu Tools > Add-ons > User Styles > 'Write New Style' and 'Edit', respectively. However, it's not obvious what CSS to use. It's unclear how to change colour of e.g. window border or window grey background (which would be beneficial for intuitive visual identification).
