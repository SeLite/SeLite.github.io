{% comment %}
 Don’t add any more 1st level menu items, neither make wording of the existing items longer. Reason: With Firefox 46.0.1 on Samsung S5 (SM-G900I) in landscape, the menu still shows up vertically. When there were more menu items at 1st level, they wouldn’t all show up on the screen. (The problem is bigger in landscape than in portrait).

This page doesn't have YAML Front Matter block (which would be between ---- and ----). Otherwise, such YAML would show up as page content this when file is included from _layouts/default.md.
This page has to be .md rather than .html, so that we can use it with Markdown Viewer add-on(see DocumentationStandard.md). Only when it's an .md file, Markdown Viewer automatically changes the local links that don't contain .md extension to contain .md extension. (Otherwise the referenced files won't open locally in Firefox.)
-->
{% endcomment %}
{% comment %}
Following turns on/off Bootstrap menu classes. It allows selite.github.io/TableOfContents to display both the Bootstrap menu, and then the following lists as a tree (below the menu) rather than as a Bootstrap menu. I've tried to turn these Bootstrap classes on/off in Javascript, but then Bootstrap menu didn't show well:
        $( function () {
            $( "#selite-menu-span li.toc-menu" ).addClass( 'dropdown' );
            $( "#selite-menu-span ul.toc-menu" ).addClass( 'dropdown-menu' );
        } );
{% endcomment %}
<!-- pageName contains "TableOfContents" and "TableOfContents" contains pageName -->
{% if include.asBootstrapMenu %}
    {% assign topLiClass = 'class="dropdown"' %}
    {% assign luClass = 'class="dropdown-menu"' %}
    {% assign caret = '<span class="caret"></span>' %}
    {% assign menuOpener= '<a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false"' %}
    {% assign menuCloser= '</a>' %}
{% else %}
    {% assign topLiClass = '' %}
    {% assign luClass = '' %}
    {% assign caret = '' %}
    {% assign menuOpener= '<span' %}
    {% assign menuCloser= '</span>' %}
{% endif %}
<li {{ topLiClass }}>
  {{ menuOpener }} data-group-page-names="./ SeleniumIDE Components  Preview">Automate{{ caret }}{{ menuCloser }}
  <ul {{ luClass }} role="menu">
    <li><a href="./">Overview</a></li>
    <li><a href="Components">Components (Install)</a></li>
    <li><a href="SeleniumIDE">SeleniumIDE</a></li>
    <li><a href="Preview">Preview (Experimental)</a></li>
  </ul>
</li>
<li {{ topLiClass }}>
  {{ menuOpener }} data-group-page-names="Settings SettingsManifests SettingsScope SettingsInterface SettingsFields SettingsLogins SettingsAPI">Settings{{ caret }}{{ menuCloser }}
  <ul {{ luClass }} role="menu">
    <li><a href="Settings">Settings</a></li>
    <li><a href="SettingsManifests">SettingsManifests</a></li>
    <li><a href="SettingsScope">SettingsScope</a></li>
    <li><a href="SettingsInterface">SettingsInterface</a></li>
    <li><a href="SettingsFields">SettingsFields</a></li>
    <li><a href="SettingsLogins">SettingsLogins</a></li>
    <li><a href="SettingsAPI">SettingsAPI</a></li>
  </ul>
</li>
<li {{ topLiClass }}>
  {{ menuOpener }} data-group-page-names="SelBlocksGlobal CustomisingSelenese ClassicSelenese EnhancedSelenese ExtraCommands">Selenese{{ caret }}{{ menuCloser }}
  <ul {{ luClass }} role="menu">
    <li><a href="ClassicSelenese">ClassicSelenese</a></li>
    <li><a href="EnhancedSelenese">EnhancedSelenese</a></li>
    <li><a href="SelBlocksGlobal">SelBlocksGlobal</a></li>
    <li><a href="ExtraCommands">ExtraCommands</a></li>
    <li><a href="CustomisingSelenese">CustomisingSelenese</a></li>
  </ul>
</li>
<li {{ topLiClass }}>
  {{ menuOpener }} data-group-page-names="DevelopmentTools InstallFromSource ExtensionSequencer JavascriptEssential JavascriptComplex JavascriptSpecial AutoCheck Bootstrap ExtensionSequencer AppsFrameworks GeneralFramework InstallFramework DotclearFramework DrupalFramework FUDforumFramework SerendipityFramework">Develop{{ caret }}{{ menuCloser }}
  <ul {{ luClass }} role="menu">
    <li><a href="DevelopmentTools">DevelopmentTools</a></li>
    <li><a href="InstallFromSource">InstallFromSource</a></li>
    <li class="divider"></li>
    <li><a href="JavascriptEssential">JavascriptEssential</a></li>
    <li><a href="JavascriptComplex">JavascriptComplex</a></li>
    <li><a href="JavascriptSpecial">JavascriptSpecial</a></li>
    <li class="divider"></li>
    <li><a href="AutoCheck">AutoCheck</a></li>
    <li><a href="Bootstrap">Bootstrap</a></li>
    <li><a href="ExitConfirmationChecker">ExitConfirmationChecker</a></li>
    <li><a href="ExtensionSequencer">ExtensionSequencer</a></li>
    <li class="divider"></li>
    <li><a href="AppsFrameworks">AppsFrameworks</a></li>
    <li><a href="GeneralFramework">GeneralFramework</a></li>
    <li><a href="InstallFramework">InstallFramework</a></li>
    <li class="divider"></li>
    <li class="dropdown-header">Application-specific:</li>
    <li><a href="DotclearFramework">DotclearFramework</a></li>
    <li><a href="DrupalFramework">DrupalFramework</a></li>
    <li><a href="FUDforumFramework">FUDforumFramework</a></li>
    <li><a href="SerendipityFramework">SerendipityFramework</a></li>
  </ul>
</li>
<li {{ topLiClass }}>
  {{ menuOpener }} data-group-page-names="HandlingData TimeStamps DataImport SQLiteImport SQLiteSpecifics">Data{{ caret }}{{ menuCloser }}
  <ul {{ luClass }} role="menu" data-placement="left">
    <li><a href="HandlingData">HandlingData</a></li>
    <li><a href="TimeStamps">TimeStamps</a></li>
    <li><a href="DataImport">DataImport</a></li>
    <li><a href="SQLiteImport">SQLiteImport</a></li>
    <li><a href="SQLiteSpecifics">SQLiteSpecifics</a></li>
  </ul>
</li>
<li {{ topLiClass }}>
  {{ menuOpener }} data-group-page-names="AboutDocumentation AddOnsThirdParty ComponentsDependants ThirdPartyIssues TroubleShooting ReportingIssues WhySeleniumIDE TestMethods TestMethodsTheory DocumentationStandard DataObjects SeleniumFlow PackagedScripts">Other{{ caret }}{{ menuCloser }}
  <ul {{ luClass }} role="menu" data-placement="left">
    <li><a href="AboutDocumentation">AboutDocumentation</a></li>
    <li><a href="AddOnsThirdParty">AddOnsThirdParty</a></li>
    <li><a href="ComponentsDependants">ComponentsDependants</a></li>
    <li><a href="ThirdPartyIssues">ThirdPartyIssues</a></li>
    <li class="divider"></li>
    <li><a href="TroubleShooting">TroubleShooting</a></li>
    <li><a href="ReportingIssues">ReportingIssues</a></li>
    <li><a href="WhySeleniumIDE">WhySeleniumIDE</a></li>
    <li class="divider"></li>
    <li class="dropdown-header">Testing-specific:</li>
    <li><a href="TestMethods">TestMethods</a></li>
    <li><a href="TestMethodsTheory">TestMethodsTheory</a></li>
    <li class="divider"></li>
    <li class="dropdown-header">Internal:</li>
    <li><a href="DocumentationStandard">DocumentationStandard</a></li>
    <li><a href="DataObjects">DataObjects</a></li>
    <li><a href="SeleniumFlow">SeleniumFlow</a></li>
    <li><a href="PackagedScripts">PackagedScripts</a></li>
  </ul>
</li>