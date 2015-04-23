---
title: SeLite AddOns for Firefox
layout: default
---

# What to get
For straightforward use of SeLite, get all its add-ons. If you only want some of them, read [AddOnsDependants](AddOnsDependants).

In addition to add-ons, see [SeleniumIDE](SeleniumIDE) for productivity tips.

## Quick download
For the easiest download get all add-ons of [SeLite collection](https://addons.mozilla.org/en-US/firefox/collections/peter-kehl/selite/?sort=name). However, that can be out of date: it receives latest releases delayed by about a week. If you depend on recent functionality, or if you'd like to run [PackagedTests](PackagedTests), see below.

Don't install SeLite together with SelBlocks (or Flow Control, GoTo or Sideflow) - for details see [SelBlocksGlobal](SelBlocksGlobal).

## Latest releases
Background: After Mozilla screens an add-on's update for security, you can get it at 'download' links below. However, the update doesn't show at add-on's 'info' page (nor at the above collection), until someone from Mozilla reviews it. If you've installed an older version of the add-on, you won't get an automatic update until then either.

So, use the _download_ links below for recent releases. **Ignore** the text: 'You should always use the latest version of an add-on.' Do not follow those _'latest version'_ links (which can be out of date).

{::options parse_block_html="true" /}

# Add-ons
<!-- Keep the following table sorted alphabetically. Using options parse_block_html="true" didn't help, so links are in HTML. -->
<table>
<tbody>
<tr>
    <td> <strong>Extension name</strong><br/>and <strong>user documentation</strong><br/>
        (if it exists)
    </td>
    <td><strong>General<br/>info</strong></td>
    <td><strong>Download</strong> <br/>
        (and<br/>
        <strong>Change log</strong>)
    </td>
    <td><strong>More details</strong><br/>
        <ul>
            <li>Selenium IDE menu</li>
            <li>Selenese reference (see also <a href="SeleniumIDE">SeleniumIDE</a> > <a href="SeleniumIDE#auto-generated-selenese-commands">Auto-generated Selenese commands</a></li>
            <li>tests (see also <a href="PackagedTests">PackagedTests</a>)</li>
            <li>source, only if there is no other documentation</li>
            <li>third party documentation</li>
            <li><em><a href="AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui">chrome://</em> URL</a> to configure via <a href="SettingsInterface">SettingsInterface</a></li><li>license (if other than GNU LGPL 3)</li>
        </ul>
        (<em><a href="AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui">chrome://</em> URLs</a> only work after you install the add-on in Firefox. See also <a href="AboutDocumentation">AboutDocumentation</a> > <a href="AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui">Firefox chrome URLs</a>)
    </td>
</tr>
<tr>
    <td><a href="AutoCheck">AutoCheck</a></td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-auto-check/'>info</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-auto-check/versions/'>download</a></td>
    <td> Configuration: <em>chrome://selite-settings/content/tree.xul?module=extensions.selite-settings.common</em> > autoCheck<code>*</code></td>
</tr>
<tr>
    <td> <a href='BootstrapLoader'>Bootstrap</a></td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-bootstrap/'>info</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/SeLite-Bootstrap/versions/'>download</a></td>
    <td> Configuration: <em>chrome://selite-settings/content/tree.xul?module=extensions.selite-settings.common</em> > bootstrappedCoreExtensions </td>
</tr>
<tr>
    <td>Clipboard And Indent</td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-clipboard-and-indent/'>info</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-clipboard-and-indent/versions'>download</a> </td>
    <td> Apache License 2 </td>
</tr>
<tr>
    <td> <a href='ExtraCommands'>Commands</a></td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-commands/'>info</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-commands/versions/'>download</a>                </td>
    <td> <ul>
        <li><a href='https://selite.googlecode.com/git/commands/src/chrome/content/reference.xml'>Selenese reference</a></li>
        <li><a href='https://code.google.com/p/selite/source/browse/#git%2Fcommands%2Fselenese-tests'>Selenese tests</a></li>
    </ul> </td>
</tr>
<tr>
    <td> DB Objects                </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-db-objects/'>info</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-db-objects/versions/'>download</a>              </td>
    <td> <ul>
        <li><a href='https://code.google.com/p/selite/source/browse/db-objects/src/chrome/content/'>source files</a></li>
        <li><a href='https://selite.googlecode.com/git/db-objects/src/chrome/content/reference.xml'>Selenese reference</a></li>
    </ul> </td>
</tr>
<tr>
<td> <a href="ExitConfirmationChecker">ExitConfirmationChecker</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-exit-confirmation-check/'>info</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-exit-confirmation-check/versions'>download</a> </td>
    <td> <ul>
        <li>Configuration: <em>chrome://selite-settings/content/tree.xul?module=extensions.selite-settings.common</em> > exitConfirmationChecker<code>*</code></li>
        <li><a href='https://code.google.com/p/selite/source/browse#git%2Fexit-confirmation-checker%2Fselenese-tests'>Selenese tests</a></li>
    </ul> </td>
</tr>
<tr>
 <td> <a href="ExtensionSequencer">ExtensionSequencer</a>        </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-extension-sequencer/'>info</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-extension-sequencer/versions/'>download</a>     </td>
    <td> <ul>
        <li><a href='http://htmlpreview.github.io/?https://github.com/selite/main/blob/master/extension-sequencer/shell-tests/tests.html'>Shell tests (list)</a></li>
        <li><a href='https://code.google.com/p/selite/source/browse/#git%2Fextension-sequencer%2Fshell-tests'>Shell tests (source)</a></li>
        <li>Apache License 2</li>
    </ul> </td>
</tr>
<tr>
    <td> Hands-on GUI              </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-hands-on-gui/'>info</a>  </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-hands-on-gui/versions/'>download</a>           </td>
    <td> Apache License 2 </td>
</tr>
<tr>
    <td> Miscellaneous             </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-miscellaneous/'>info</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-miscellaneous/versions/'>download</a>           </td>
    <td> <ul>
         <li><a href='https://code.google.com/p/selite/source/browse/misc/src/chrome/content/extensions/core-extension.js'>Source</a></li>
         <li>Source (offline): <em>chrome://selite-misc/content/extensions/core-extension.js</em></li>
         <li><a href='https://code.google.com/p/selite/source/browse/misc/#misc%2Fselenese-tests'>Selenese tests</a></li>
         <li>Javascript tests: <em>chrome://selite-misc/content/javascript_test_runner.html?chrome://selite-misc/content/javascript-tests/test.js</em></li>
     </ul>          </td>
</tr>
<tr>
    <td> Run All Favorites         </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-run-all-favorites/'>info</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-run-all-favorites/versions/'>download</a>            </td>
    <td> <ul>
        <li>Not backwards compatible with Favorites (Selenium IDE), only forward compatible (see <i>info</i>).</li>
        <li>It requires <a href='https://addons.mozilla.org/en-US/firefox/addon/favorites-selenium-ide/'>Favorites (Selenium IDE)</a>.</li>
        <li>MPL License 1.1</li>
    </ul> </td>
</tr>
<tr>
    <td> <a href="SelBlocksGlobal">SelBlocksGlobal</a></td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-selblocks-global/'>info</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/SeLite-SelBlocks-Global/versions/'>download</a>        </td>
    <td> <ul>
        <li><a href='https://addons.mozilla.org/en-US/firefox/addon/selenium-ide-sel-blocks/'>SelBlocks summary</a></li>
        <li><a href='http://refactoror.wikia.com/wiki/Selblocks_Reference'>SelBlocks reference</a> (most applies, for differences see SelBlocksGlobal)</li>
        <li><a href='http://sel-blocks-global.selite.googlecode.com/git/src/chrome/content/reference.xml'>Selenese reference</a></li>
        <li><a href='https://code.google.com/p/selite/source/browse?repo=sel-blocks-global#git%2Fselenese-tests'>Selenese tests</a></li>
        <li>MPL License 1.1</li>
    </ul> </td>
</tr>
<tr>
    <td> <a href='SettingsOverview'>Settings</a>                </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-settings/'>info</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-settings/versions/'>download</a>                </td>
    <td> <ul>
        <li>Options > SeLite Settings for this test suite</li>
        <li>Configuration: <em>chrome://selite-settings/content/tree.xul</em></li>
        <li>Registration: <em>chrome://selite-settings/content/tree.xul?register</em></li>
        <li>Resolving per folder: <em>chrome://selite-settings/content/tree.xul?selectFolder</em></li>
        <li><a href='https://code.google.com/p/selite/source/browse/settings/src/chrome/content/SeLiteSettings.js'>source</a></li>
        <li>GUI is under GNU GPL 3; API is under GNU LGPL 3</li>
    </ul> </td>
</tr>
<tr>
    <td> SQLite Connection Manager </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-sqlite-connection-mg/'>info</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/SeLite-SQLite-Connection-Mg/versions/'>download</a>     </td>
    <td>&#160;</td>
</tr>
<tr>
    <td> TestCase Debug Context   </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/selite-testcase-debug-conte/'>info</a> </td>
    <td> <a href='https://addons.mozilla.org/en-US/firefox/addon/SeLite-TestCase-Debug-Conte/versions/'>download</a>      </td>
    <td> Apache License 2</td>
</tr>
</tbody>
</table>

# Cutting edge
If you're eager to use the development versions, apply [InstallFromSource](InstallFromSource). Also, installing the add-ons that way may be faster than downloading them one by one. Additionally, the source contains [PackagedTests](PackagedTests), which serve as active documentation. If you use a <a href='http://git-scm.com/downloads'>GIT client</a>, it gives you easy access to future development versions.
