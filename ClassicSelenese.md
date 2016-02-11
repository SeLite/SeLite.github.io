---
title: (syntax)
layout: default
---
{% include links %}

# Summary and scope
These notes on Selenese syntax are on top of [Selenium documentation](http://docs.seleniumhq.org/docs/02_selenium_ide.jsp). See [EnhancedSelenese](EnhancedSelenese) on how SeLite improves it.

# Auto-generated Selenese commands #
Selenese [commands][command] are defined in these primary forms: <code>xyz, <strong>get</strong>Xyz, <strong>is</strong>Xyz</code> or <code><strong>is</strong>Xyz<strong>Present</strong></code>. Selenium auto-generates their variations (listed below).

Selenium IDE shows the original reference for both the primary and auto-generated commands. However, the online reference contains only the primary commands. So if you'd like to locate them online (or in the source), use the following. (See also `loadSeleniumCommands()` in Selenium IDE's `treeView.js`.<!-- TODO link to chrome & SE IDE Github-->)

<table class="table">
<thead>
<tr>
    <td><strong markdown="span">Primary [command]</strong><br/> (as listed both online and in Reference tab; same as Javascript implementation function, unless mentioned otherwise)</td>
    <td><strong markdown="span">Auto-generated [commands][command]</strong><br/>(listed in Reference tab only, but not online)</td>
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

# Selenese parameters (`target` and `value`)
Selenese [commands][command] accept up to two parameters: `target` and `value`.

* `target` is often a locator (an expression that identifies an element). It can also be a Javascript expression (for `getEval`).
* One or multiple parts of `target` or `value` can be Javascript expressions, each enclosed within back ticks \`...\`. A few variations of back tick notation exist. See [EnhancedSelenese](EnhancedSelenese).
* If `target` is a locator, `value` is usually the expected or new value, which is compared or entered into element identified by `target`. `value` can also be a name of a stored variable, or something else.
* [Selenium Core reference](http://release.seleniumhq.org/selenium-core/1.0.1/reference.html) > [Element Locators](http://release.seleniumhq.org/selenium-core/1.0.1/reference.html#locators) is handy (if you installed Selenium IDE, see it offline in Firefox at {{chromeUrl}} _chrome://selenium-ide/content/selenium-core/reference.html#locators_).
* Command references can call these parameters something more relevant/specific. Then the first one stands for `target` and the second one (if used) for `value`.

## XPath
Full XPath locators start with `xpath=` or with //. Usually an XPath expression starts with //, with which you don't need to use `xpath=`. However, some SeLite [commands][command] (like `clickRandom` and `selectRandom`) only accept XPath as the locator, but they require it not to contain any leading `xpath=` prefix (whether the XPath starts with // or not).

See resources on XPath:

* [MDN](https://developer.mozilla.org/en-US/docs/Web/XPath)
* [W3C](http://www.w3.org/TR/xpath/)
* [XPath functions supported by Firefox](https://developer.mozilla.org/en-US/docs/XPath/Functions)
* [XPath siblings](http://stackoverflow.com/questions/365750/xpath-sibling-conditional-testing)

## UI-Element mappings
UI Mapping (or 'UI-Element mapping') defines a mapping between meaningful names of elements on webpages, and the elements themselves. Element locators are in forms

* `ui=semanticPageName::semanticElementName(...)` or
* `ui=semanticPageName::semanticElementName(...)->xpathOffsetLocator`.

Find a basic example at Selenium Documentation > [Test Design Considerations](http://www.seleniumhq.org/docs/06_test_design_considerations.jsp) > [UI Mapping](http://www.seleniumhq.org/docs/06_test_design_considerations.jsp#ui-mapping) (which is written in Java, but SeLite frameworks define UI Mappings in Javascript.) See also a more [detailed example](https://github.com/SeleniumHQ/selenium/blob/master/javascript/selenium-core/scripts/ui-map-sample.js) (or at {{chromeUrl}} _chrome://selenium-ide/content/selenium-core/scripts/ui-map-sample.js_). Read its [detailed reference](http://htmlpreview.github.io/?https://github.com/SeleniumHQ/selenium/blob/master/javascript/selenium-core/scripts/ui-doc.html). If you've installed Selenium IDE, access the same reference offline through Selenium IDE menu > Help > UI-Element Documentation (or at _chrome://selenium-ide/content/selenium-core/scripts/ui-doc.html_).

# Variables

## Stored variables
Some Selenese [commands][command] store variables, to be used by later steps. [SelBlocksGlobal](SelBlocksGlobal) manages scope of such variables. When inside a [SelBlocksGlobal](SelBlocksGlobal) function, you can only use stored variables set in that function (or passed as parameters to it). The local scope also means: if you set a stored variable within a Selenese function and the same stored variable exists in the caller scope (that invoked the current function), the variable in the caller scope won't be affected.

<!-- TODO Put an explicit rule first, then details. Merge/link to EnhancedSyntax: -->Parameters of Selenese commands can access stored variables as `${name-of-the-variable}`. Those get replaced by the value of the variable. However, if a command processes the parameter as a Javascript expression (e.g. `storeEval, getEval` or when using [EnhancedSelenese](EnhancedSelenese)), and if the variable contains an array/object or a non-numeric string (possibly with an apostrophe or quotation mark), then replacement of `${name-of-the-variable}` won't work robustly. For those cases use `storedVars.name-of-the-variable` or `storedVars['name-of-the-variable']`. See also [EnhancedSelenese](EnhancedSelenese).

## Javascript variables
Sometimes you want a _global_ variable that spreads across Selenese functions (which stored variables can't). Use _native_ Javascript variables for it. Set them using [command] `getEval` with `target` being: `variable1=valueOrExpression, variable2=valueOrExpression....`

Don't use `storeEval` for that - it sets a stored variable, which is local to current Selenese [function].

# Limitations of getEval, storeEval
[Command] `getEval` (and derived commands like `storeEval` - as per [Auto-generated Selenese commands](#auto-generated-selenese-commands) above) etc. don’t like new line string literals `"\n"` or `'\n'` (or any string literals that contain them). Then they generate a confusing error `unterminated string literal`. Use `String.fromCharCode(10)` or `'\\u000A'` (as per [a suggestion](https://code.google.com/p/selenium/issues/detail?id=1816#c7)) instead.