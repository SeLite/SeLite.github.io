---
title: Settings Interface
layout: default
---

# Accessing the interface of SeLiteSettings #
Two most frequent use cases are available in Selenium IDE menu > Options. They link to <i>tree.xul</i> and <i>tree.xul?selectFolder</i> (see below). Changes are immediate: if you change a field's value, or delete a set, it's saved right then.

SeLiteSettings' configuration interface is within Firefox at a special urls starting with [_chrome://_](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui). That way it's usable even without Selenium IDE. The URLs are
| <i>chrome://selite-settings/content/tree.xul</i>                                   | to manage Firefox profile-based set(s) for all registered modules |
|:-----------------------------------------------------------------------------------|:------------------------------------------------------------------|
| <i>chrome://selite-settings/content/tree.xul?module=full-module-name</i>           | to manage set(s) for a given module |
| <i>chrome://selite-settings/content/tree.xul?prefix=prefix-of-full-module-name</i> | to manage set(s) of all modules whose name starts with given prefix |
| <i>chrome://selite-settings/content/tree.xul?selectFolder</i>                      | to select a folder for which to review any fields |
| <i>chrome://selite-settings/content/tree.xul?folder=/full/path/to/folder</i>       | to review effective configuration for test suites in that folder, based on any applicable sets or manifests as per [SettingsScope](SettingsScope) |
| <i>chrome://selite-settings/content/tree.xul?register</i><br><font color='FFFFFF'>thispreventsthecontenttobreakintomulltiplelinesxxxxxxxxxxxxxxxxxxxxxxxxxxx</font>                          <table><thead><th> to register a module manually (from its javascript file). You need this only if you design or import a custom configuration module that is not packaged as a Core extensions of Selenium IDE. Alternatively you may use your own Core extension and [SettingsAPI](SettingsAPI) to register your module automatically. </th></thead><tbody></tbody></table>

Per-folder view colours fields based on where the value(s) come from:<br>
<ul><li>module definition (field default)<br>
</li><li>Firefox profile-based configuration set<br>
</li><li>values manifest</li></ul>

<h1>Reloading databases</h1>
SeLiteSettings adds three buttons to Selenium IDE. They re-load one or two of test DB, app DB or vanilla DB (as per the table below). These buttons do the full job on their own only if your web app uses SQLite as its DB. Otherwise apply [DataImport](DataImport).<br>
<br>
See [TestMethodsTheory](TestMethodsTheory) for definition of test and app DB. Vanilla DB serves as a snapshot of app DB, which you can revert to when the test got out of sync with the app, because of a bug in your test, timeout etc.<br>
<br>
<table><thead><th>                                                                                                </th><th> <b>Source DB</b> </th><th> <b>Target DB</b> </th><th> <b>Extra target DB</b> </th></thead><tbody>
<tr><td> <img src='https://selite.googlecode.com/git/settings/src/chrome/skin/classic/reload_test.png' />             </td><td> App       </td><td> Test      </td><td>                 </td></tr>
<tr><td> <img src='https://selite.googlecode.com/git/settings/src/chrome/skin/classic/reload_vanilla_and_test.png' /> </td><td> App       </td><td> Test      </td><td> Vanilla         </td></tr>
<tr><td> <img src='https://selite.googlecode.com/git/settings/src/chrome/skin/classic/reload_app_and_test.png' />     </td><td> Vanilla   </td><td> Test      </td><td> App             </td></tr></tbody></table>

You should pause tests while using these buttons, otherwise the test script or application may modifying their DB files. Beware of background web processes (Ajax or CRON) - wait until they finish. Otherwise you may need to stop the application (e.g. by shutting down Tomcat/JBoss, Apache or WEBrick). If the DB file is on a network filesystem, it may not lock properly.<br>
<br>
<h2>Permissions</h2>
Reloading databases requires your local account (which runs Firefox) to have access to delete the web app DB file and to create new files in the web app DB folder. That works when the app runs in  locally and you started it yourself, or when it is under your home folder. Otherwise your account needs to have the access granted (e.g. via Linux/Mac OS groups or SeLinux setfacl - see [DrupalFramework](DrupalFramework)).<br>
<br>
<h1>Typing strings 'null' or 'undefined'</h1>
This is unlikely, but possible. If a freetype field has a non-null defined value (possibly empty string), then you can type in strings 'null' or 'undefined' and they will be accepted as those strings. They won't get converted to Javascript null or undefined. Use a special column 'Null/Undefine' for that.<br>
<br>
If a freetype field is Javascript null or undefined, you can click at its cell and change it. If you you don't change it and you hit Enter, that leaves the field as Javascript null or undefined, rather than a string 'null' or 'undefined'. So, if  you'd like to enter a string literal 'null' or 'undefined', enter something else first and then change it to 'null' or 'undefined' again.