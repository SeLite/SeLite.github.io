---
layout: default
rss: http://www.feed43.com/selite-third_party_issues.xml
---
* TOC
{:toc}

# Your vote is significant #
Below is a list of issues (or pull requests) in Selenium IDE, Firefox and third party software that complicate maintenance and progress of SeLite. They also restrict usability of Selenium IDE. Many of them were reported by other people, but they affect SeLite, too. Some of those defects and requests even have fixes proposed, but not accepted by the third party.

Only your vote will help to get attention of those third parties. Please, make the difference: Vote for these issues.

## Your privacy is essential ##
If you vote for an issue, by default you will receive automated emails on changes to that issue. However, you can deactivate those notifications. Indeed, voting for an issue doesn't add your email address to any forums.

# Where to vote

## Quick links
Use the following links to register with third parties, disable email notifications (unless you want them), list the issues/pull requests and vote for them:

<!--
Update the following links whenever you update the detailed list.
How to get the links for GitHub: I couldn"t make it search by pairs of [repository, issue #]. Therefore
https://github.com/search?q=repo%3Arefactoror%2FSelBlocks+label%3Aquestion+author%3Apeter-kehl&ref=searchresults&type=Issues&utf8=%E2%9C%93. To edit that search, visit this link and then follow "Advanced search" link from that screen.
-->
<script type="text/javascript">
function goToYourGitHubComments( repositoryUser, repositoryProject ) {
    var username=prompt('What is your GitHub username?');
    if(username) {
        window.location= 'https://github.com/' +escape(repositoryUser)+ '/' +escape(repositoryProject)+ '/issues?utf8=✓&q=open+commenter%3A' + escape(username);
    }
}
</script>
| **Third party**      | **Hosted at** | **Registration**                                                                     | **Email preferences**                                                                         | **Issues relevant to SeLite** | **Your votes (or comments)** |
|:---------------------|:--------------|:-------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------|:------------------------------|:-------------------|
| _Projects on GitHub_ | _GitHub_      | [GitHub registration](https://github.com/join)                           | [GitHub notifications](https://github.com/settings/notifications) | (see below) | https://github.com/search?type=Issues&utf8=✓&q=commenter%3A**your-github-user-name** <br/><a href="#" onclick="var username=prompt('What is your GitHub username?'); if(username) { window.location= 'https://github.com/search?type=Issues&utf8=✓&q=commenter%3A' +escape(username); }">your comments</a> |
| Selenium             | GitHub        |             |                                               |  (see below) | https://github.com/seleniumHQ/selenium/issues?utf8=✓&q=commenter%3A**your-github-user-name** <br/><a href="#" onclick="goToYourGitHubComments('seleniumHQ', 'selenium')">your comments</a> |
| Selblocks            | GitHub        |  |  | [Selblocks issues](https://github.com/search?q=repo%3Arefactoror%2FSelBlocks+label%3Aquestion+author%3Apeter-kehl&ref=searchresults&type=Issues&utf8=✓) | https://github.com/refactoror/SelBlocks/issues?utf8=✓&q=commenter%3A**your-github-user-name** <br/><a href="#" onclick="goToYourGitHubComments('refactoror', 'Selblocks')">your comments</a> |
| PURE                 | GitHub        |  |  | [PURE pull request](https://github.com/pure/pure/pull/20) | https://github.com/pure/pure/issues?utf8=✓&q=commenter%3A**your-github-user-name** <br/><a href="#" onclick="goToYourGitHubComments('pure', 'pure')">your comments</a> |
| _Projects outside of GitHub_ | | | | | | |
| Mozilla              | Mozilla       | [Mozilla registration](https://bugzilla.mozilla.org/createaccount.cgi)              | [Mozilla preferences](https://bugzilla.mozilla.org/userprefs.cgi?tab=email)                  | [Mozilla issues](https://bugzilla.mozilla.org/buglist.cgi?quicksearch=ALL+bug_id%3A396966%2C406629%2C962861%2C852837%2C837961%2C627808%2C929703%2C932578%2C891774%2C278536%2C1031985%2C1051632%2C1108132%2C1096135%2C1071816%2C1247476) | [Mozilla votes](https://bugzilla.mozilla.org/page.cgi?id=voting/user.html) |
| NetBeans             | NetBeans      | [NetBeans registration](https://netbeans.org/people/new)                            | [NetBeans preferences](https://netbeans.org/bugzilla/userprefs.cgi?tab=email)            | [NetBeans issues](https://netbeans.org/bugzilla/buglist.cgi?quicksearch=ALL%20bug_id%3A237640%2C238942%2C244329%2C234888%2C%2C238121%2C240529%2C238691%2C238942) | [NetBeans votes](https://netbeans.org/bugzilla/page.cgi?id=voting/user.html) |
{: .table}

Please, vote even if an issue is closed (with comments like _doesn't meet a 'feature bar'_). Subscribe to [XML RSS feed](http://www.feed43.com/selite-third_party_issues.xml) on these issues.

## Selenium, Markdown Viewer, SelBlocks, PURE (on GitHub)
[Registration with GitHub](https://github.com/join) is easy.

There is no voting for issues/pull requests on GitHub. Instead, add a thumbs up ![thumbs up](https://assets-cdn.github.com/images/icons/emoji/unicode/1f44d.png) reaction. Watch out, however: reactions are per-comment. Choose an existing comment that supports fixing the problem rather than avoids it. (For reactions click at the smiley face near the top right corner of a comment.) Do not 'vote' by Star button at the top right corner. (It doesn't star the issue/pull request itself. Instead, it stars the whole repository.)

By default, you will get notifications for issues that you comment on. After you commented on an issue, you may want to unfollow it by 'Unsubscribe' button on the right. See also your [notification settings](https://github.com/settings/notifications) and [repositories that you are watching](https://github.com/watching).

## Mozilla and NetBeans
Mozilla and NetBeans also require you to register in order to vote. It takes a few steps, but it's important: those products have many more other issues and voters (when compared to Selenium), so we need many votes to expedite the issues. Register yourself at [NetBeans registration](https://netbeans.org/people/new) and [Mozilla registration](https://bugzilla.mozilla.org/createaccount.cgi). Submit those forms, then check your email inbox and confirm your registration by following a link that you'll receive.

The actual voting is in two steps. Click at 'vote' right of Importance (for Mozilla) or right of Priority (for NetBeans). That takes you to a page with a checkbox next to Vote For This Bug. Activate the checkbox and submit by clicking at button Change My Votes near the bottom-left.

You can turn off any notifications at [Mozilla preferences](https://bugzilla.mozilla.org/userprefs.cgi?tab=email) or [NetBeans preferences](https://netbeans.org/bugzilla/userprefs.cgi?tab=email) > button Disable All Mail and then button Submit Changes at the bottom.

# Detailed list of issues/pull requests
<!-- Use exact issue names (including typos!), or shorten them with "..." but only at the end. That eases the navigation. Keep them sorted in order of importance. -->

| *Issue*                                                                                                                          | *Third party*   | *Reason*                           |
| [tree.inputField's type as autocomplete fails](https://bugzilla.mozilla.org/show_bug.cgi?id=1247476)                             | Mozilla         | Usable Clipboard And Indent |
| [Selenium IDE chrome/content/formats/html.js has an incorrect regex](https://github.com/SeleniumHQ/selenium/issues/1636)         | Selenium        | Reliable testing |
| [Selenium IDE chrome/content/formats/html.js to preserve indented...](https://github.com/SeleniumHQ/selenium/issues/1546)        | Selenium        | Readable test cases |
| [Enable use with XHTML](https://github.com/pure/pure/pull/20)                                                                    | PURE            | Exporting templatedXML
| [Source code highlighted, but Reference is empty](https://github.com/esdoc/esdoc/issues/222)                                     | ESDoc           | Generating developer documentation |
| [Support ECMAScript 6 Template Literals](https://github.com/SeleniumHQ/selenium/issues/1662)                                     | Selenium        | Compact tests |
| [Base URL Should Allow Path](https://github.com/SeleniumHQ/selenium/issues/1550)                                                 | Selenium        | Practical testing and integration |
| [Refactor TestCase.debugContext to have a class on its own](https://github.com/SeleniumHQ/selenium/issues/1537)                  | Selenium        | Stable API of [SelBlocksGlobal](SelBlocksGlobal) |
| [Core extensions are loaded 2x - document this, or prevent it](https://github.com/SeleniumHQ/selenium/issues/1549)               | Selenium        | Robust core extensions |
| [verify* should show the diff](https://github.com/SeleniumHQ/selenium/issues/1538)                                               | Selenium        | Robust tests. <br>See [ExitConfirmationChecker](ExitConfirmationChecker) &gt; [Details](ExitConfirmationChecker#details) |
| [Details of error reporting in user/plugin javascript](https://github.com/SeleniumHQ/selenium/pull/61)                           | Selenium        | Productive development |
| [IDE: alert() fails in a modified test case](https://github.com/SeleniumHQ/selenium/issues/1768)                                 | Selenium        | Reliable automation |
| [IDE: a subsequent call to open() fails with multiprocessed Firefox (e10s)](https://github.com/SeleniumHQ/selenium/issues/1769)  | Selenium | Major Firefox Compatibility |
| [XPath 2.0](https://bugzilla.mozilla.org/show_bug.cgi?id=396966)                                                                 | Mozilla         | Robust tests.<br>(For now see <a href='https://developer.mozilla.org/en-US/docs/XPath/Functions'>supported functions</a>.) |
| [Breakpoint triggers on code that doesn't run...](https://bugzilla.mozilla.org/show_bug.cgi?id=1051632)                          | Mozilla         | Productive development |
| [Sidebars  (history, bookmarks) should not have maximum width](https://bugzilla.mozilla.org/show_bug.cgi?id=406629)              | Mozilla         | GUI usability |
| [Allow sidebar to be selected and turned on/off independently](https://bugzilla.mozilla.org/show_bug.cgi?id=962861)              | Mozilla         | GUI usability |
| [Support for( var value of array ) {...} loop](https://netbeans.org/bugzilla/show_bug.cgi?id=237640)                             | NetBeans        | Cleaner code |
| [ICU-based Intl.DateTimeFormat implementation...](https://bugzilla.mozilla.org/show_bug.cgi?id=852837)                           | Mozilla         | Timestamps across timezones |
| [Add support for IANA time zone names to internationalization API](https://bugzilla.mozilla.org/show_bug.cgi?id=837961)          | Mozilla         | Timestamps across timezones |
| [Selblocks and SelBlocksGlobal](https://github.com/refactoror/SelBlocks/issues/4)                                                | Selblocks       | Joined effort |
| [Report user extension/plugin error stack](https://github.com/SeleniumHQ/selenium/issues/1548)                                   | Selenium        | Debugging |
| [safe_alert() fails at UI element startup in Selenium IDE](https://github.com/SeleniumHQ/selenium/issues/1535)                   | Selenium        | Design of UI element locators |
| [UI element test cases should be run even after Selenium IDE startup](https://github.com/SeleniumHQ/selenium/issues/1536)        | Selenium        | Design of UI element locators |
| [Incorrect Error Reporting for Javascript 1.7 keywords](https://netbeans.org/bugzilla/show_bug.cgi?id=238942)                    | NetBeans        | Simpler & robust code using 'const' keyword |
| [Allow search minidialog and highlighting for multiple tabs/windows...](https://netbeans.org/bugzilla/show_bug.cgi?id=244329)    | NetBeans        | Code navigation |
| [Code fold at indention level](https://netbeans.org/bugzilla/show_bug.cgi?id=234888)                                             | NetBeans        | Code navigation |
| [Highlight NaN, Infinity and undefined](https://netbeans.org/bugzilla/show_bug.cgi?id=238121)                                    | NetBeans        | Editing |
| [Show version awaiting review in detail page...](https://bugzilla.mozilla.org/show_bug.cgi?id=627808)                            | Mozilla         | Distributing new versions<br> (Can't be voted on yet) |
| [Optionally wrap long lines in the debugger](https://bugzilla.mozilla.org/show_bug.cgi?id=1108132)                               | Mozilla         | Productive development |
| [Don't collapse empty {}](https://netbeans.org/bugzilla/show_bug.cgi?id=240529)                                                  | NetBeans        | Code navigation |
| [JavaScript editor uses camelCase navigation even when disabled](https://netbeans.org/bugzilla/show_bug.cgi?id=238691)           | NetBeans        | Code navigation |
| [&lt;treechildren tooltip="_child"&gt; doesn't work](https://bugzilla.mozilla.org/show_bug.cgi?id=929703)                        | Mozilla         | Robust code |
| [Allow sidebars to be different widths](https://bugzilla.mozilla.org/show_bug.cgi?id=932578)                                     | Mozilla         | Flexible testing |
| [JavaScript Console reports exceptions as warnings when using "use strict"](https://bugzilla.mozilla.org/show_bug.cgi?id=725468) | Mozilla         | Robust development |
| ["use strict"; violations only logged at LOG level from AddonManager.jsm](https://bugzilla.mozilla.org/show_bug.cgi?id=1096135)  | Mozilla         | Robust development |
| [nsTreeView and TreeView.setCellText() is either badly...](https://bugzilla.mozilla.org/show_bug.cgi?id=891774)                  | Mozilla         | Robust SeLite Settings |
| [Tree documentation: setCellText and redrawing](https://bugzilla.mozilla.org/show_bug.cgi?id=278536)                             | Mozilla         | Robust SeLite Settings |
| [console reports syntax error for valid json fetched via jquery.ajax](https://bugzilla.mozilla.org/show_bug.cgi?id=1031985)      | Mozilla         | Cleaner log in Browser Console |
| [Support loading BOMless UTF-8 text/plain files from file: URLs](https://bugzilla.mozilla.org/show_bug.cgi?id=1071816)           | Mozilla         | Cleaner log in Browser Console |
| [Confusing text: Be careful with old versions!...](https://bugzilla.mozilla.org/show_bug.cgi?id=1239898)                         | Mozilla         | Clear download instructions<br> (Can't be voted on yet) |
| [Regex search in any folder tree with an empty file fails](https://netbeans.org/bugzilla/show_bug.cgi?id=257897)                 | NetBeans        | Smooth development |
| [Documentation and handling of Selenium.prototype.getXYZ functions...](https://github.com/SeleniumHQ/selenium/issues/1635)       | Selenium        | Reliable API |
| [Backslashes get reduced to half](https://github.com/SeleniumHQ/selenium/issues/2215) | Selenium | Usability |
{: .table #issues}

<!--
<tr><td> <a href='https://code.google.com/archive/p/selenium/issues/2706'>Base URL inconsistent behavior (IDE)</a>        </td><td> Selenium    </td><td> Flexible testing </td></tr>
<tr><td> <a href='https://code.google.com/p/selenium/issues/detail?id=1816'>[IDE] JS regex replace for line break does not work...</a> </td><td> Selenium </td><td> Robust and expressive <tr><td> <a href='http://code.google.com/p/selenium/issues/detail?id=3028'>Keyboard shortcut to Selenium IDE</a>                            </td><td> Selenium    </td><td> GUI usability </td></tr>
-->