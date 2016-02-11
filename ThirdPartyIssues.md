---
layout: default
---

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

Filter multiple GitHub pull requests by commit # and 'OR': https://github.com/Thiht/markdown-viewer/issues?utf8=%E2%9C%93&q=213d5645af8+OR++f6269e7a3+
-->
<script type="text/javascript">
function goToYourGitHubComments( repositoryUser, repositoryProject ) {
    var username=prompt('What is your GitHub username?');
    if(username) {
        window.location= 'https://github.com/' +escape(repositoryUser)+ '/' +escape(repositoryProject)+ '/issues?utf8=✓&q=open+commenter%3A' + escape(username);
    }
}
</script>
| **Third party**   | **Hosted at** | **Registration**                                                                     | **Email preferences**                                                                         | **Issues relevant to SeLite** | **Your votes (or comments)** |
|:------------------|:--------------|:-------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------|:------------------------------|:-------------------|
| _Projects on GitHub_ | _GitHub_ | [GitHub registration](https://github.com/join)                           | [GitHub notifications](https://github.com/settings/notifications) | (see below) | https://github.com/search?type=Issues&utf8=✓&q=commenter%3A**your-github-user-name** <br/><a href="#" onclick="var username=prompt('What is your GitHub username?'); if(username) { window.location= 'https://github.com/search?type=Issues&utf8=✓&q=commenter%3A' +escape(username); }">your comments</a> |
| Selenium      | GitHub |                            |                                  |  (not migrated yet) | https://github.com/seleniumHQ/selenium/issues?utf8=✓&q=commenter%3A**your-github-user-name** <br/><a href="#" onclick="goToYourGitHubComments('seleniumHQ', 'selenium')">your comments</a> |
| Markdown Viewer | GitHub   |  |  | [Markdown Viewer feature](https://github.com/Thiht/markdown-viewer/pull/39) | https://github.com/Thiht/markdown-viewer/issues?utf8=✓&q=commenter%3A**your-github-user-name** <br/><a href="#" onclick="goToYourGitHubComments('Thiht', 'markdown-viewer')">your comments</a> |
| Selblocks     | GitHub     |  |  | [Selblocks issues](https://github.com/search?q=repo%3Arefactoror%2FSelBlocks+label%3Aquestion+author%3Apeter-kehl&ref=searchresults&type=Issues&utf8=✓) | https://github.com/refactoror/SelBlocks/issues?utf8=✓&q=commenter%3A**your-github-user-name** <br/><a href="#" onclick="goToYourGitHubComments('refactoror', 'Selblocks')">your comments</a> |
| Mozilla       | Mozilla     | [Mozilla registration](https://bugzilla.mozilla.org/createaccount.cgi)              | [Mozilla preferences](https://bugzilla.mozilla.org/userprefs.cgi?tab=email)                  | [Mozilla issues](https://bugzilla.mozilla.org/buglist.cgi?quicksearch=ALL+bug_id%3A396966%2C406629%2C962861%2C852837%2C837961%2C627808%2C929703%2C932578%2C891774%2C278536%2C1031985%2C1051632%2C1108132%2C1096135%2C1071816%2C1247476) | [Mozilla votes](https://bugzilla.mozilla.org/page.cgi?id=voting/user.html) |
| _Projects outside of GitHub_ | | | | | | |
| NetBeans     | NetBeans   | [NetBeans registration](https://netbeans.org/people/new)                            | [NetBeans preferences](https://netbeans.org/bugzilla/userprefs.cgi?tab=email)            | [NetBeans issues](https://netbeans.org/bugzilla/buglist.cgi?quicksearch=ALL%20bug_id%3A237640%2C238942%2C244329%2C234888%2C%2C238121%2C240529%2C238691%2C238942) | [NetBeans votes](https://netbeans.org/bugzilla/page.cgi?id=voting/user.html) |
{: .table}

Please, vote even if an issue is closed (with comments like _doesn't meet a 'feature bar'_). Stay updated: subscribe to get updates about [new third party issues](http://www.feed43.com/8850141255642605.xml) (via RSS) and vote for them in future.

## Selenium, Markdown Viewer and SelBlocks (on GitHub)
[Registration with GitHub](https://github.com/join) is easy.

There is currently no voting for issues/pull requests on GitHub. Instead, add comments like '+1'. (Do not use Star button at the top right corner for voting: It doesn't star the issue/pull request itself - it stars the whole repository instead.)

By default, you will get notifications for issues that you comment on. After you commented on an issue, you may want to unfollow it by 'Unsubscribe' button on the right. See also your [notification settings](https://github.com/settings/notifications) and [repositories that you are watching](https://github.com/watching).

## Mozilla and NetBeans
Mozilla and NetBeans also require you to register in order to vote. It takes a few steps, but it's important: those products have many more other issues and voters (when compared to Selenium), so we need many votes to expedite the issues. Register yourself at [NetBeans registration](https://netbeans.org/people/new) and [Mozilla registration](https://bugzilla.mozilla.org/createaccount.cgi). Submit those forms, then check your email inbox and confirm your registration by following a link that you'll receive.

The actual voting is in two steps. Click at 'vote' right of Importance (for Mozilla) or right of Priority (for NetBeans). That takes you to a page with a checkbox next to Vote For This Bug. Activate the checkbox and submit by clicking at button Change My Votes near the bottom-left.

You can turn off any notifications at [Mozilla preferences](https://bugzilla.mozilla.org/userprefs.cgi?tab=email) or [NetBeans preferences](https://netbeans.org/bugzilla/userprefs.cgi?tab=email) > button Disable All Mail and then button Submit Changes at the bottom.

# Detailed list of issues/pull requests
<!-- Use exact issue names, or shorten them with "..." but only at the end. That eases the navigation. Keep them sorted in order of importance. -->
<table class="table">
<thead><th> Issue                                                                                               </th><th> Third party </th><th> Reason                           </th></thead><tbody>

<!-- -->
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=1247476'>tree.inputField's type as autocomplete fails</a> </td><td>Mozilla</td><td>Usable Clipboard And Indent</td></tr>
<tr><td> <a href='https://github.com/SeleniumHQ/selenium/issues/1546'>Selenium IDE chrome/content/formats/html.js to preserve indented...</a>    </td><td> Selenium    </td><td> Readable test cases.<!-- There is a workaround at <a href="SeleniumIDE">SeleniumIDE</a> > <a href="SeleniumIDE#hands-on-gui">Hands-on GUI</a> and <a href="SeleniumIDE#clipboard-and-indent">Clipboard And Indent</a>--> </td></tr>
<tr><td> <a href='https://github.com/SeleniumHQ/selenium/issues/1550'>Base URL Should Allow Path</a>                  </td><td> Selenium    </td><td> Practical testing and integration </td></tr>
<tr><td> <a href='https://github.com/SeleniumHQ/selenium/issues/1537'>Refactor TestCase.debugContext to have a class on its own</a>             </td><td> Selenium    </td><td> Stable API of <a href="SelBlocksGlobal">SelBlocksGlobal</a> </td></tr>
<tr><td> <a href='https://github.com/SeleniumHQ/selenium/issues/1549'>Core extensions are loaded 2x</a>               </td><td> Selenium    </td><td> Robust core extensions </td></tr>
<tr><td> <a href='https://github.com/SeleniumHQ/selenium/issues/1538'>verify* should show the diff</a>               </td><td> Selenium    </td><td> See <a href="ExitConfirmationChecker">ExitConfirmationChecker</a> &gt; <a href='ExitConfirmationChecker#details'>Details</a> </td></tr>
<!-- -->
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=396966'>Support XPath 2.0</a>                               </td><td> Mozilla     </td><td> Robust tests.<br>(For now see <a href='https://developer.mozilla.org/en-US/docs/XPath/Functions'>supported functions</a>.) </td></tr>
<tr><td><a href="https://github.com/Thiht/markdown-viewer/pull/39">Support for local .md links...</a></td> <td>Markdown Viewer</td><td>Offline access to documentation</td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=1051632'>Breakpoint triggers on code that doesn't...</a>    </td><td> Mozilla     </td><td> Productive development </td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=406629'>Sidebars should not have maximum width</a>          </td><td> Mozilla     </td><td> GUI usability </td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=962861'>Allow sidebar to be selected and turned on/off independently</a>      </td><td> Mozilla     </td><td> GUI usability </td></tr>
<tr><td> <a href='https://netbeans.org/bugzilla/show_bug.cgi?id=237640'>Support for( var value of array ) {...} loop</a>   </td><td> NetBeans   </td><td> Cleaner code </td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=852837'>ICU-based Intl.DateTimeFormat implementation...</a> </td><td> Mozilla     </td><td> Timestamps across timezones </td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=837961'>Add support for IANA time zone names to internationalization API</a>   </td><td> Mozilla     </td><td> Timestamps across timezones </td></tr>
<tr><td> <a href='https://github.com/refactoror/SelBlocks/issues/4'>Selblocks and SelBlocksGlobal </a>                     </td><td> Selblocks   </td><td> Joined effort </td></tr>

<!-- -->
<tr><td> <a href='https://github.com/SeleniumHQ/selenium/issues/1548'>Report user extension/plugin error stack</a>             </td><td> Selenium    </td><td> Debugging </td></tr>
<tr><td> <a href='https://github.com/SeleniumHQ/selenium/issues/1535'>safe_alert() fails at UI element startup in Selenium IDE</a>  </td><td> Selenium    </td><td> Design of UI element locators </td></tr>
<tr><td> <a href='https://github.com/SeleniumHQ/selenium/issues/1536'>UI element test cases should be run even after Selenium IDE startup</a> </td><td> Selenium    </td><td> Design of UI element locators </td></tr>
<!-- -->
<tr><td> <a href='https://netbeans.org/bugzilla/show_bug.cgi?id=238942'>...Javascript 1.7 keywords</a>             </td><td> NetBeans   </td><td> Simpler & robust code using 'const' keyword </td></tr>
<tr><td> <a href='https://netbeans.org/bugzilla/show_bug.cgi?id=244329'>Allow search minidialog and highlighting for multiple tabs/windows...</a> </td><td> NetBeans   </td><td> Code navigation </td></tr>
<tr><td> <a href='https://netbeans.org/bugzilla/show_bug.cgi?id=234888'>Code fold at indention level</a>                  </td><td> NetBeans   </td><td> Code navigation </td></tr>
<tr><td> <a href='https://netbeans.org/bugzilla/show_bug.cgi?id=238121'>Highlight NaN, Infinity and undefined</a>          </td><td> NetBeans   </td><td> Editing </td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=627808'>Show version awaiting review in detail page...</a>  </td><td> Mozilla     </td><td> Distributing new versions </td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=1108132'>Browser ToolBox (Debugger) should...</a>           </td><td> Mozilla     </td><td> Productive development </td></tr>
<tr><td> <a href='https://netbeans.org/bugzilla/show_bug.cgi?id=240529'>Don't collapse empty {}</a>                        </td><td> NetBeans   </td><td> Code navigation </td></tr>
<tr><td> <a href='https://netbeans.org/bugzilla/show_bug.cgi?id=238691'>JavaScript editor uses camelCase navigation even when disabled</a>       </td><td> NetBeans   </td><td> Code navigation </td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=929703'>&lt;treechildren tooltip="_child"&gt; doesn't work</a>    </td><td> Mozilla     </td><td> Robust code</td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=932578'>Allow sidebars to be different widths</a>           </td><td> Mozilla     </td><td> Flexible testing </td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=1096135'>"use strict"; violations only logged at LOG level from AddonManager.jsm</a> </td><td> Mozilla </td><td>  Less distracted development </td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=891774'>nsTreeView and TreeView.setCellText() is either badly...</a>         </td><td> Mozilla     </td><td> Robust SeLite Settings </td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=278536'>Tree documentation: setCellText and redrawing</a>                    </td><td> Mozilla     </td><td> Robust SeLite Settings </td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=1031985'>console reports syntax error for valid json fetched via jquery.ajax</a> </td><td> Mozilla </td><td> Cleaner log in Browser Console </td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=1071816'>Support loading BOMless UTF-8 text/plain files from file: URLs</a> </td><td> Mozilla    </td><td> Cleaner log in Browser Console </td></tr>
<tr><td> <a href='https://bugzilla.mozilla.org/show_bug.cgi?id=1239898'>Confusing text: Be careful with old versions! ...</a></td><td> Mozilla </td><td> Clear download instructions </td></tr>
<tr><td> <a href='https://netbeans.org/bugzilla/show_bug.cgi?id=257897'>Regex search in any folder tree with an empty file fails</a> </td><td> NetBeans </td><td>Smooth development </td></tr>

<!--
<tr><td> <a href='http://code.google.com/p/selenium/issues/detail?id=2706'>Base URL inconsistent behavior (IDE)</a>        </td><td> Selenium    </td><td> Flexible testing </td></tr>
<tr><td> <a href='https://code.google.com/p/selenium/issues/detail?id=1816'>[IDE] JS regex replace for line break does not work...</a> </td><td> Selenium </td><td> Robust and expressive <tr><td> <a href='http://code.google.com/p/selenium/issues/detail?id=3028'>Keyboard shortcut to Selenium IDE</a>                            </td><td> Selenium    </td><td> GUI usability </td></tr>
-->