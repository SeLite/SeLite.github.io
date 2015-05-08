---
title: Selenium IDE notes/tips
layout: default
---

# Summary and scope
These notes about [Selenium IDE](http://seleniumhq.org/projects/ide) are on top of its [standard documentation](http://docs.seleniumhq.org/docs/02_selenium_ide.jsp). This assumes that you have installed all SeLite [AddOns](AddOns). If you develop test scripts, frameworks or plugins, see also [DevelopmentTools](DevelopmentTools).

# Auto-generated Selenese commands #
Selenese commands are defined in these primary forms: <code>xyz, <strong>get</strong>Xyz, <strong>is</strong>Xyz</code> and <code><strong>is</strong>Xyz<strong>Present</strong></code>. Selenium auto-generates their variations (listed below).

Selenium IDE shows the original reference for both the primary and auto-generated actions. However, the online reference contains only the primary actions. So if you'd like to locate them online (or in the source), use the following. (See also `loadSeleniumCommands()` in Selenium IDE's `treeView.js`.)

<table>
<thead>
<tr>
    <td><strong>Primary command</strong><br/> (as listed both online and in Reference tab; same as Javascript implementation function, unless mentioned otherwise)</td>
    <td><strong>Auto-generated commands</strong><br/>(listed in Reference tab only, but not online)</td>
</tr>
</thead>
<tbody>
<tr>
    <td> xyz<br>(implemented by function <strong>do</strong>Xyz)</td>
    <td> xyz<strong>AndWait</strong>                       </td>
</tr>
<tr>
    <td>&#160;</td>
    <td>&#160;</td>
</tr>
<tr>
    <td><strong>assert</strong>Xyz</td>
    <td><strong>verify</strong>Xyz</td>
</tr>
<tr>
    <td>&#160;</td>
    <td>&#160;</td>
</tr>
<tr>
    <td><strong>get</strong>Xyz<br><strong>is</strong>Xyz (other than <strong>is</strong>Xyz<strong>Present</strong>)<br> (both are rarely useful on their own<br> - use generated commands instead)</td>
    <td> <strong>assert</strong>Xyz<br><strong>assertNot</strong>Xyz</td>
</tr>
<tr>
    <td>&#160;</td>
    <td> <strong>verify</strong>Xyz<br><strong>verifyNot</strong>Xyz</td>
</tr>
<tr>
    <td>&#160;</td>
    <td> <strong>store</strong>Xyz</td>
</tr>
<tr>
    <td>&#160;</td>
    <td> <strong>waitFor</strong>Xyz<br><strong>waitForNot</strong>Xyz</td>
</tr>
<tr>
    <td>&#160;</td>
    <td>&#160;</td>
</tr>
<tr>
    <td> <strong>is</strong>Xyz<strong>Present</strong><br>(rarely useful on its own<br> - use generated commands instead) </td>
    <td> <strong>assert</strong>Xyz<strong>Present</strong> <br><strong>assert</strong>Xyz<strong>NotPresent</strong> </td>
</tr>
<tr>
    <td>&#160;</td>
    <td> <strong>verify</strong>Xyz<strong>Present</strong> <br><strong>verify</strong>Xyz<strong>NotPresent</strong> </td>
</tr>
<tr>
    <td>&#160;</td>
    <td> <strong>store</strong>Xyz<strong>Present</strong> </td>
</tr>
<tr>
    <td>&#160;</td>
    <td> <strong>waitFor</strong>Xyz<strong>Present</strong> <br><strong>waitFor</strong>Xyz<strong>NotPresent</strong>  </td>
</tr>
</tbody></table>

# Selenese parameters (Target and Value)
Selenese commands accept up to two parameters: Target and Value.

* Target is often a locator (an expression that identifies an element). The whole target can also be a Javascript expression (for `getEval`).
* One or multiple parts of Target or Value can be Javascript expressions, each enclosed within back ticks \`...\`. A few variations of back tick notation exist. See [EnhancedSyntax](EnhancedSyntax).
* If Target is a locator, Value is usually the expected or new value, which is compared or entered into element identified by Target. Value can also be a name of a stored variable, or something else.
* [Selenium Core reference](http://release.seleniumhq.org/selenium-core/1.0.1/reference.html) > [Element Locators](http://release.seleniumhq.org/selenium-core/1.0.1/reference.html#locators) is handy (if you installed Selenium IDE, see it offline in Firefox at [_chrome://_ URL](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) _chrome://selenium-ide/content/selenium-core/reference.html#locators_).

## XPath
Full XPath locators start with `xpath=` or with //. Usually an XPath expression starts with //, with which you don't need to use `xpath=`. However, some SeLite commands (like `clickRandom` and `selectRandom`) only accept XPath as the locator, but they require it not to contain any leading `xpath=` prefix (whether the XPath starts with // or not).

See resources on XPath:

* [MDN](https://developer.mozilla.org/en-US/docs/Web/XPath)
* [W3C](http://www.w3.org/TR/xpath/)
* [XPath functions supported by Firefox](https://developer.mozilla.org/en-US/docs/XPath/Functions)
* [XPath siblings](http://stackoverflow.com/questions/365750/xpath-sibling-conditional-testing)

## UI-Element mappings
UI Mapping (or 'UI-Element mapping') defines a mapping between meaningful names of elements on webpages, and the elements themselves. Element locators are in forms

* `ui=semanticPageName::semanticElementName(...)` or
* `ui=semanticPageName::semanticElementName(...)->xpathOffsetLocator`.

Find a basic example at Selenium Documentation > [Test Design Considerations](http://www.seleniumhq.org/docs/06_test_design_considerations.jsp) > [UI Mapping](http://www.seleniumhq.org/docs/06_test_design_considerations.jsp#ui-mapping) (which is written in Java, but SeLite frameworks define UI Mappings in Javascript.) See also a more [detailed example](https://github.com/SeleniumHQ/selenium/blob/master/javascript/selenium-core/scripts/ui-map-sample.js) (or at [_chrome://_ URL](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) _chrome://selenium-ide/content/selenium-core/scripts/ui-map-sample.js_). Read its [detailed reference](http://htmlpreview.github.io/?https://github.com/SeleniumHQ/selenium/blob/master/javascript/selenium-core/scripts/ui-doc.html). If you've installed Selenium IDE, access the same reference offline through Selenium IDE menu > Help > UI-Element Documentation (or at _chrome://selenium-ide/content/selenium-core/scripts/ui-doc.html_).

# Variables

## Stored variables
Some Selenese actions store variables, to be used by further actions. [SelBlocksGlobal](SelBlocksGlobal) manages scope of such variables. When inside a [SelBlocksGlobal](SelBlocksGlobal) function, you can only use stored variables set in that function (or passed as parameters to it). The local scope also means: if you set a stored variable within a Selenese function and the same stored variable exists in the caller scope (that invoked the current function), the variable in the caller scope won't be affected.

Parameters of Selenese actions can access stored variables as `${name-of-the-variable}`. Those get replaced by the value of the variable. However, if the action processes the parameter as a Javascript expression (e.g. `storeEval, getEval` or when using [EnhancedSyntax](EnhancedSyntax)), and if the variable contains an array/object or a non-numeric string (possibly with an apostrophe or quotation mark), then replacement of `${name-of-the-variable}` won't work robustly. For those cases use `storedVars.name-of-the-variable` or `storedVars['name-of-the-variable']`. See also [EnhancedSyntax](EnhancedSyntax).

## Javascript variables
Sometimes you want a 'global' variable that spreads across Selenese functions (which stored variables can't). Use 'direct' Javascript variables for it. Set them using command/action `getEval` with the target being: `variable1=valueOrExpression, variable2=valueOrExpression....`

Don't use command `storeEval` for that - it sets a stored variable, which is local.

# Limitations of getEval, storeEval
Command `getEval` (and derived commands like `storeEval` - as per [Auto-generated Selenese commands](#auto-generated-selenese-commands) above) etc. don’t like new line string literals `"\n"` or `'\n'` (or any string literals that contain them). Then they generate a confusing error _'unterminated string literal'_. Use `String.fromCharCode(10)` instead.

# Tips on GUI usability

## Add .html extension to files
When saving a test case or a test suite, Selenium IDE doesn't add `'.html'` extension. So, add `.html` yourself, which will let you identify the file more easily.

## Using multiple Selenium IDEs in parallel
A running Firefox instance can show only one standard Selenium IDE window. Yet, viewing/editing different test cases in multiple Selenium IDE windows (at the same time) increases productivity. It's beneficial for restructuring scripts (e.g. into Selenese functions), or as a reference for test cases. Several ways exist for it, varying in intuitiveness, simplicity and accessibility. Some involve multiple running instances of Firefox, with separate profiles.

For robust access modify all test cases and test suites only in primary Selenium IDE window. Alternatively, edit different test cases (or sets of them) in different Selenium IDEs. Either way, beware that sometimes other Selenium IDEs don't pick up the change when saved (even if you switch between test cases in those other Selenium IDEs) or they notice it only later. So if you modify a test case in one chosen Selenium IDE and you use other Selenium IDEs as its read-only reference, it may be out of date.

### Auxiliary Selenium IDEs inside browser
This shows one (standard) Selenium IDE detached from Firefox browser windows. Other Selenium IDEs are inside browser windows, but they may look standard, too. Compared to the next method, this

* (+) is simpler to set up, to start and to maintain
* (+) involves less maintenance: it runs only one Firefox instance (with one profile)
* (+) is more convenient: shared bookmarks, window history, add-ons etc.
* (+-) shares history of recent files in Selenium IDE; however, it gets overwritten by the instance closed as the last one
* (-) can't run scripts in auxiliary Selenium IDEs (they target their own window/tab). If you modify a script there without saving it and your run a command that changes the current page, your changes will be lost! So you should use auxiliary Selenium IDEs primarily to view scripts (for reference). Auxiliary Selenium IDEs don't load any [SettingsManifests](SettingsManifests) neither any [Settings](Settings) sets associated with test suites. (That prevents potential conflicts due to the fact that any auxiliary Selenium IDEs share Selenium Core scope along with the standalone Selenium IDE, if any). Therefore they don't load any extensions through [BootstrapLoader](BootstrapLoader), either.

Steps

1. Install [Stylish](https://addons.mozilla.org/en-US/firefox/addon/stylish)
2. Install [Hide Tab Bar With One Tab](https://addons.mozilla.org/en-US/firefox/addon/hide-tab-bar-with-one-tab/)
3. Install [Hide Navigation Bar](https://addons.mozilla.org/en-us/firefox/addon/hide-navigation-bar/) (for usage see below)
4. Install [per-tab coloured menu text](https://userstyles.org/styles/110010/selenium-ide-per-tab-coloured-menu-text):
   <a href="https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshots/110010_after.jpeg?r=1424730593"><img src="https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshot_thumbnails/110010_after.jpeg?r=1424730593"/></a>
5. Restart Firefox
6. Start standalone Selenium IDE (by Ctrl+Alt+S, or Firefox menu Tools > Selenium IDE)
7. Display auxiliary Selenium IDEs. For each
 1. open a new Firefox window
 2. open one of [_chrome://_ URLs](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui)
    * _chrome://selenium-ide/content/selenium-ide.xul#GREEN_
    * _chrome://selenium-ide/content/selenium-ide.xul#BLUE_
    * _chrome://selenium-ide/content/selenium-ide.xul#RED_
    * _chrome://selenium-ide/content/selenium-ide.xul#WHITE_
 3. if you have already opened <em>chrome://selenium-ide/content/selenium-ide.xul</em> and later you add or change the hash part (<em>#GREEN, #BLUE</em>, <em>#RED</em> or <em>#WHITE</em>), it won't take effect (even after you hit Enter) until you refresh the URL e.g. by F5 key (which will lose any modifications)
 4. hide Firefox navigation bar by pressing F2 (you may need to press it twice)
  * (+-) it applies to the current window and any new windows later, but not to other existing Firefox windows
  * (-) however, after a Firefox restart it applies to all windows; then press F2 to show navigation bar where you want it

In auxiliary IDEs

* optionally, press F11 for full screen mode (this is probably beneficial only if you have more than two physical screens)
* (-) if you hide Firefox menu, it often doesn't show up when you press Alt key. Then you need to click at the very top of the browser window (which shows <em>testCaseName (testCaseFileName.html) - Selenium IDE X.Y.Z - Mozilla Firefox</em>) and then press Alt.
* (-) pressing F5 (which reloads Selenium IDE), or closing Selenium IDE tab/window with 'x' icon, applies without any confirmation about unsaved test cases/test suite. So it's the safest not to modify tests in auxiliary IDEs.

Side note: There is also Firefox menu > View > Sidebar > Selenium IDE. However, that Selenium IDE sidebar has restricted width. Like Auxiliary Selenium IDEs, Selenium IDE in a sidebar doesn't load any [SettingsManifests](SettingsManifests), any [Settings](Settings) sets, neither any extensions through [BootstrapLoader](BootstrapLoader). Please, vote at [ThirdPartyIssues](ThirdPartyIssues) > [Sidebars (history, bookmarks) should not have maximum width](https://bugzilla.mozilla.org/show_bug.cgi?id=406629) so that it gets fixed.

### Multiple standard Selenium IDEs
This shows multiple Selenium IDEs, detached from Firefox browser windows. Compared to the previous method, this
* (-) needs multiple running Firefox instances (and profiles). Each profile each needs an installation of Selenium IDE; ones that run scripts also need their own installations of SeLite [AddOns](AddOns) and any other extensions.
* (+) can run multiple scripts in parallel with separate sessions (cookies)

To set up

1. Create Firefox profiles, one per Selenium IDE: Follow [Setting up extension development environment (MDN)](https://developer.mozilla.org/en-US/Add-ons/Setting_up_extension_development_environment) > [Development profile](https://developer.mozilla.org/en-US/Add-ons/Setting_up_extension_development_environment#Development_profile).
2. Start multiple Firefox instances, one per profile, with command line parameters <em>-no-remote</em> and (<em>-P ProfileName</em>).
3. Install Selenium IDE in each profile. Install SeLite [AddOns](AddOns) any other extensions in profiles where you will run scripts.
4. Mark the second and successive profiles to identify Selenium IDEs:
  * Visually by menu text colour
    1. Install [Stylish](https://addons.mozilla.org/en-US/firefox/addon/stylish)
    2. Restart Firefox
    3. Install styles for the second and further Selenium IDEs:
       * [green](https://userstyles.org/styles/109886/selenium-ide-green-menu-text){:style="color: green;"}:<br/><a href='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshots/109886_after.jpeg?r=1422833554'><img src='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshot_thumbnails/109886_after.jpeg?r=1422833554' /></a>
       * [blue](https://userstyles.org/styles/110005/selenium-ide-blue-menu-text){:style="color: blue;"}':<br/> <a href='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshots/110005_after.jpeg?r=1422833463'><img src='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshot_thumbnails/110005_after.jpeg?r=1422833463' /></a>
       * [red](https://userstyles.org/styles/110006/selenium-ide-red-menu-text){:style="color: red;"}:<br/> <a href='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshots/110006_after.png?r=1430731358'><img src='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshot_thumbnails/110006_after.png?r=1430731358' /></a>
       * [white](https://userstyles.org/styles/110620/selenium-ide-white-menu-text){:style="color: white; background: grey;"}:<br/> <a href='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshots/110620_after.png?r=1430730143'><img src='https://df6a.https.cdn.softlayer.net/80DF6A/static.userstyles.org/style_screenshot_thumbnails/110620_after.png?r=1430730143' /></a>
    4. You can switch between styles in Firefox menu Tools > Add-ons > User Styles.
  * By language (this applies to the whole Firefox instance, rather than just Selenium IDE, which can make it difficult to use!)
    1. Install [Simple Locale Switcher](https://addons.mozilla.org/en-US/firefox/addon/simple-locale-switcher/)
    2. Restart Firefox
    3. Install different language packs through Firefox toolbar > 'Simple Locale Switcher' green button > 'Get more language packs...'