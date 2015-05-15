---
layout: default
---
{% include links %}

# Status #
This add-on is a work in progress. Currently it works only for Selenese commands `type` and `typeRandom`.

# Overview #
Modern web pages assist you to prevent unintended loss of the data that you entered. When you edit a form but you don't submit it and then you try to close the tab (or the window or the browser), or you try to navigate away from the screen (following links or typing a new URL), the page asks you to confirm your intention. In Firefox it says 'This page is asking you to confirm that you want to leave - data you have entered may not be saved.' and it gives you a choice of two buttons: 'Stay on Page' and 'Leave Page'. (You won't be able to close the tab/window or even Firefox unless you click at one of those buttons, or unless you close Firefox forcibly). The application asks for that confirmation in a handler for [window.onbeforeunload()](https://developer.mozilla.org/en-US/docs/WindowEventHandlers.onbeforeunload).

This is not limited to unsubmitted forms. The web application may have two or more pages which store user's inputs/actions in the session, and only on the further pages the application saves/submits that data. If you try to close or navigate away from the website after it stored some data in the session but before it saved/submitted the data, it should confirm your intention.

Also, say you have an application that attracts your attention (e.g. an alert from a calendar or an email client). Then the application may use the same confirmation method to increase the likelihood that you read the alert.

ExitConfirmationChecker helps your [scripts][script] to validate that the application asks for this confirmation exactly when it should. (It doesn't add any Selenese commands.)

# What it does for you #
This add-on suppresses the confirmation dialog activated from `window.onbeforeunload()`. If that's all you need, use mode `ignored`. The other two active modes `includeRevertedChanges` and `skipRevertedChanges` also verify (or assert) whether the confirmation popup would show up (or would not show up) as it should.

## Out-of-the-box ##
Out-of-the-box ExitConfirmationChecker only handles

  * unsubmitted modified forms, and
  * standard HTML inputs (no WYSIWYG editors), and
  * inputs modified via Selenium or SeLite commands: `type` and `typeRandom` (from SeLite Commands) and `select` only for now; not `check` or `click` yet, but not modified by a human (between the [script] runs), and
  * pages that are direct results of Selenese actions (i.e. not results of redirect or manual user navigation), and
  * any inputs as relevant for confirmation. You may have inputs that don't need confirmation when leaving the page (e.g. search-like fields), but by default ExitConfirmationChecker expects the confirmation if there was a change to any input. You can override that.<!-- TODO: how to override? Provide functions and/or filters.-->

### Reverted changes (or zero changes) ###
A [script] may select a new value for a field and then change it back to its original value, or type content in a free-type field same as its original content (or type something else and then type the original content back). The application may keep a list of original values of the fields and then treat such a field as unchanged. Otherwise it treats such a field as unchanged. ExitConfirmationChecker can validate both behaviours. You can configure which behaviour your application uses.

# Configuration #
This uses [Settings](Settings) with standard module `extensions.selite-settings.common` and fields `exitConfirmationCheckerAssert` and `exitConfirmationCheckerMode`. `exitConfirmationCheckerAssert` indicates whether the defects in showing (or not showing) the confirmation should result in Selenese assertion failure (like failing any `assertXyz` command). Otherwise such defects only result in verification failure (like failing any `verifyXyz` command). `exitConfirmationCheckerMode` can be

  * `inactive`: no effect, confirmation popups show up as application drives them (and they will stop Selenium IDE from running the rest of the [script])
  * `ignored`: no validation, but hide any confirmation popups. Good if you want your [script] to run without validating the popups.
  * `includeRevertedChanges`: validate confirmation, expect confirmation for reverted changes, don't show any confirmation popups
  * `skipRevertedChanges`: validate confirmation, expect no confirmation for reverted changes, don't show any confirmation popups

## Limitations ##
Here are a few rules of thumb:

  * If you run Selenese commands one by one, beware that the validation is delayed by one command.
  * Don't have any commands that modify inputs or (re)load the page at the end of a [suite] (or a [case], if you run the case on its own).
  * If you don't use `exitConfirmationCheckerAssert`, ignore error logs unless they increase the error count.

### Details ###
This validates effect of a command only just before running the next command. However, it uses configuration present for that next command. This can get highly confusing when running multiple [suites][suite] via SeLite Run All Favorites, if they have different configuration and the last command of a suite (other than the last suite) modifies an input or (re)loads the page.

If you run  a whole [case] (or [suite]), you don't set `exitConfirmationCheckerAssert`, and the previous command fails ExitConfirmationChecker validation, then ExitConfirmationChecker logs an extra error. That's because of a workaround for [ThirdPartyIssues](ThirdPartyIssues) > [verify\* should show the diff](https://code.google.com/p/selenium/issues/detail?id=1092). Otherwise, when you run a single Selenese command (by double-clicking) and the command fails ExitConfirmationChecker validation, there would be no message in the log at all. Please vote for that issue and for other [ThirdPartyIssues](ThirdPartyIssues).

<!--If your page is a result of a redirect, then you need to call `getEval | _SeLiteExitConfirmationChecker.overrideOnBeforeUnload()` before the commmand that causes the redirect. TODO implement: Have an optional parameter to indicate number of Selenese commands before the redirect; this is useful if there are structural commands in between, e.g. if/else, for/while...'>-->

# Tests #
Follow [PackagedScripts](PackagedScripts) for [Selenese tests](https://code.google.com/p/selite/source/browse#git%2Fexit-confirmation-checker%2Fselenese-tests) of Exit Confirmation Checker. See source of `form.html`, `SeLiteSettingsValues.txt` and the actual tests. Test cases with name starting with `negative` are supposed to fail: see [PackagedScripts](PackagedScripts) > [Negative tests that fail](PackagedScripts#negative-tests-that-fail).<!-- @TODO put into folder selenese-tests-negative? See PackagedScripts-->