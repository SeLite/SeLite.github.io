---
layout: default
---
{% include links %}

# Expect complexity #
This technology works with third party components and many variables. You can reduce amount of misunderstanding (for you and others) by providing and following clear and exact steps. Do not assume or simplify what seems obvious, duplicated or unnecessary.

## Got stuck? ##
When you read documentation, and things don't work:

  * stop, switch to doing something else (the less related the better), get back to it later or some other day
  * read or check 'backwards': from the last parts to the earlier ones
  * print it
  * read a print out at a different place
  * discuss it with someone in person
  * ask someone else to follow instructions in his/her environment, or observe him/her doing it in your environment.

Do the same when you write a report about a potential issue.

# Versions of components #
Have Firefox and Selenium IDE that are supported by current SeLite. See [Overview](./) > [Compatibility](./#compatibility). Get current versions of all SeLite add-ons - as per [Components(Components) > [Latest releases](Components#latest-releases). (If you only get add-ons that you need, you may not be able to [run packaged scripts](#run-packaged-scripts)). For unreleased functionality use {{navCuttingEdge}}.

# Loading Selenium plugins #
Check whether packaged Selenium extensions start correctly: Open Selenium IDE > menu Options > Options... > Plugins. Verify that each plugin is enabled and with no error message (in red). If there is an error, inspect [Browser Console](#browser-console). (That screen doesn't show error details in [SeleniumIDE](SeleniumIDE) > [auxiliary Selenium IDEs inside browser](SeleniumIDE#auxiliary-selenium-ides-inside-browser).) TODO centralise with DevelopmentTools.

By default, Selenium IDE disables plugins that fail on start, and it keeps them disabled on subsequent restarts of Selenium IDE. That's even if the problems get resolved - Selenium IDE won't load such plugins, until you enable them. So you may want to disable that in the options above. See also [DevelopmentTools](DevelopmentTools) > [Selenium IDE settings](DevelopmentTools#selenium-ide-settings).

# Narrow down the problem #
This may help you to figure out the issue yourself. Even if you don't solve it, you'll have collected information that will assist others to help you. In order to locate the issue you need to (as applicable)

  * reload your [script DB] from [app DB] and re-run the [script]
  * eliminate extras
    * disable all other Firefox extensions and plugins (including  well-known ones like Firebug). Only leave extensions required to run your script.
    * minimise any custom Selenium extensions
    * cut down your script
    * replicate the issue against local `.html` page(s), simplified as much as possible
    * replace SeLite-specific commands (e.g. `typeRandom`) with their Selenium alternatives and see whether your locators work.

## Browser Console ##
If Selenium IDE log doesn't give much information about the problem, you need a log from Firefox itself. Visit Firefox URL _about:config_. Set preference `devtools.chrome.enabled` to `true` (by double-clicking). Restart Firefox and go to its menu > Tools > Web Developer > Browser Console. Does it show any errors or warnings? Re-check the console after you start Selenium IDE and when you run scripts.

## Separate Firefox profile ##
If you don't want to modify your existing Firefox profile, [create a new one](https://developer.mozilla.org/en-US/Add-ons/Setting_up_extension_development_environment#Development_profile) (with no spaces in its name). Enable [Browser Console](#browser-console). Add any necessary non-SeLite extensions, until the problem appears. Alternatively, you could also [duplicate the existing profile](http://kb.mozillazine.org/Moving_your_profile_folder) (which is stored as a folder) and narrow down the problem in that copy.

# Run packaged scripts #
If you use {{navCuttingEdge}}, it also gives you [PackagedScripts](PackagedScripts). Otherwise get the [scripts][script] from SeLite source as per [InstallFromSource](InstallFromSource).

Disable all Selenium extensions other than ones supported by SeLite. Run scripts as per [PackagedScripts](PackagedScripts). Then enable other Selenium extensions and see if they break the scripts.

# Defects in third party components #
If the cause is in Firefox, Selenium IDE or a third party component, submit a report in the respective bug tracking system (see also [ThirdPartyIssues](ThirdPartyIssues)). If the third party confirms the problem and it is likely to affect other SeLite users, report it to SeLite, too, so that its admins add a link to that issue to [ThirdPartyIssues](ThirdPartyIssues). Then other users can star it (vote for it), which could motivate the third party to solve your problem.

# Report the issue #
If the problem is not caused by third party or custom extensions, you have something to report. Follow [ReportingIssues](ReportingIssues).