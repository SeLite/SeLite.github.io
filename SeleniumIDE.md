---
title: (GUI notes/tips)
layout: default
---
{% include links %}

# Summary and scope
These notes are about usability of [Selenium IDE](http://seleniumhq.org/projects/ide) GUI. See [ClassicSelenese](ClassicSelenese) and [EnhancedSelenese](EnhancedSelenese) regarding Selenese syntax. If you develop [scripts][script], frameworks or plugins, see also [DevelopmentTools](DevelopmentTools).

# Add .html extension to files
When saving a [case] or a [suite], Selenium IDE doesn't add `'.html'` extension. So, add `.html` yourself, which will let you identify the file more easily.

# Hands-on GUI and Clipboard And Indent
[AddOns](AddOns) > 'Hands-on GUI' and 'Clipboard And Indent' enable

* in-place editing of Selenese commands and their parameters (`target` and `value`), right in the commands list
* indentation of commands (suitable especially for [SelBlocksGlobal](SelBlocksGlobal) functions and structured commands)
* productivity keyboard shortcuts
* clipboard sharing between Selenium IDE and other applications

# Using multiple Selenium IDEs in parallel
A running Firefox instance can show only one standard Selenium IDE window. Yet, viewing/editing different [cases][case] in multiple Selenium IDE windows (at the same time) increases productivity. It's beneficial for restructuring scripts (e.g. into Selenese functions), or as a reference for cases. Several ways exist for it, varying in intuitiveness, simplicity and accessibility. Some involve multiple running instances of Firefox, with separate profiles.

For robust access modify all cases and [suites][suite] only in primary Selenium IDE window. Alternatively, edit different cases (or sets of them) in different Selenium IDEs. Either way, beware that sometimes other Selenium IDEs don't pick up the change when saved (even if you switch between cases in those other Selenium IDEs) or they notice it only later. So if you modify a [case] in one chosen Selenium IDE and you use other Selenium IDEs as its read-only reference, it may be out of date.

## Auxiliary Selenium IDEs inside browser
This shows one (standard) Selenium IDE detached from Firefox browser windows. Other Selenium IDEs are inside browser windows, but they may look standard, too. Compared to the next method, this

* (+) is simpler to set up, to start and to maintain
* (+) involves less maintenance: it runs only one Firefox instance (with one profile)
* (+) is more convenient: shared bookmarks, window history, add-ons etc.
* (+-) shares history of recent files in Selenium IDE; however, it gets overwritten by the instance closed as the last one
* (-) can't run scripts in auxiliary Selenium IDEs (they target their own window/tab). If you modify a script there without saving it and your run a command that changes the current page, your changes will be lost! So you should use auxiliary Selenium IDEs primarily to view scripts (for reference). Auxiliary Selenium IDEs don't load any [SettingsManifests](SettingsManifests) neither any [Settings](Settings) sets associated with [suites][suite]. (That prevents potential conflicts due to the fact that all auxiliary Selenium IDEs share [Core scope] along with the standalone Selenium IDE, if any). Therefore they don't load any extensions through [BootstrapLoader](BootstrapLoader), either.

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
 2. open one of {{chromeUrl}}s
    * _chrome://selenium-ide/content/selenium-ide.xul#GREEN_
    * _chrome://selenium-ide/content/selenium-ide.xul#BLUE_
    * _chrome://selenium-ide/content/selenium-ide.xul#RED_
    * _chrome://selenium-ide/content/selenium-ide.xul#WHITE_
 3. if you have already opened _chrome://selenium-ide/content/selenium-ide.xul_ and later you add or change the hash part (_#GREEN, #BLUE, #RED_ or _#WHITE_), it won't take effect (even after you hit Enter) until you refresh the URL e.g. by F5 key (which will lose any modifications)
 4. hide Firefox navigation bar by pressing F2 (you may need to press it twice)
  * (+-) it applies to the current window and any new windows later, but not to other existing Firefox windows
  * (-) however, after a Firefox restart it applies to all windows; then press F2 to show navigation bar where you want it

In auxiliary IDEs

* optionally, press F11 for full screen mode (this is probably beneficial only if you have more than two physical screens)
* (-) if you hide Firefox menu, it often doesn't show up when you press _Alt_ key. Then you need to click at the very top of the browser window (which shows `testCaseName (testCaseFileName.html) - Selenium IDE X.Y.Z - Mozilla Firefox`) and then press _Alt_.
* (-) pressing F5 (which reloads Selenium IDE), or closing Selenium IDE tab/window with 'x' icon, applies without any confirmation about unsaved case(s) or suite. The safest way is not to modify scripts in auxiliary IDEs.

Side note: There is also Firefox menu > View > Sidebar > Selenium IDE. However, that Selenium IDE sidebar has restricted width. Like Auxiliary Selenium IDEs, Selenium IDE in a sidebar doesn't load any [SettingsManifests](SettingsManifests), any [Settings](Settings) sets, neither any extensions through [BootstrapLoader](BootstrapLoader). Please, vote at [ThirdPartyIssues](ThirdPartyIssues) > [Sidebars (history, bookmarks) should not have maximum width](https://bugzilla.mozilla.org/show_bug.cgi?id=406629) so that it gets fixed.

## Multiple standard Selenium IDEs
This shows multiple Selenium IDEs, detached from Firefox browser windows. Compared to the previous method, this
* (-) needs multiple running Firefox instances (and profiles). Each profile each needs an installation of Selenium IDE; ones that run scripts also need their own installations of SeLite [AddOns](AddOns) and any other extensions.
* (+) can run multiple scripts in parallel with separate sessions (cookies)

To set up

1. Create Firefox profiles, one per Selenium IDE: Follow [Setting up extension development environment (MDN)](https://developer.mozilla.org/en-US/Add-ons/Setting_up_extension_development_environment) > [Development profile](https://developer.mozilla.org/en-US/Add-ons/Setting_up_extension_development_environment#Development_profile).
2. Start multiple Firefox instances, one per profile, with command line parameters `-no-remote` and (`-P ProfileName`).
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