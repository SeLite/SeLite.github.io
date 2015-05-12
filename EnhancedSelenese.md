---
title: Enhanced Selenese syntax
layout: default
---

# Overview
When compared to [ClassicSelenese](ClassicSelenese), automation scripts with SeLite are shorter and clearer.
 [SelBlocksGlobal](SelBlocksGlobal) enhances syntax of [ClassicSelenese](ClassicSelenese) > [Selenese parameters target and value](ClassicSelenese#selenese-parameters-target-and-value). It allows those expressions to

* conveniently access stored variables through `$storedVariableName` notation and
* use Javascript within \`...\`

# `$storedVariableName` notation
This syntax enables Selenese commands and structures that evaluate their parameter(s) as Javascript to access stored variables as `$storedVariableName`. It's for

* `getEval` (and its auto-generated variations as per [ClassicSelenese](ClassicSelenese) > [Auto-generated Selenese commands](ClassicSelenese#auto-generated-selenese-commands)),
* Javascript within \`...\` (and its variations) and
* [SelBlocksGlobal](SelBlocksGlobal) structured commands that evaluate one or both of their parameters as Javascript. The commands and their parameters that support this are
<!-- this lists commands that invoke expandStoredVars|evalWithExpandedStoredVars|cascadeElseIf|isMatchingCatch|handleCommandError|loadVars|dropToLoop -->
  * `target` parameter of: `skipNext, gotoIf, if, elseIf, throw, catch, while, for, continue, break, return`
  * `value` parameter of:
    * `forEach, loadJsonVars, loadXmlVars`
    * `call` - only on right side of function parameter assignments

See also [SelBlocksGlobal](SelBlocksGlobal) > [Accessing stored variables](SelBlocksGlobal#accessing-stored-variables). 

# Javascript within \`...\` without special prefix (cast to a string) #
This notation allows you to pass results of one or multiple Javascript expressions (each enclosed within a pair of back ticks \`...\`) to Selenese commands in their parameter (`target` or `value`). It evaluates any Javascript code in `target` or `value` that is between a pair of \`...\`. Then it converts the result to a string (excluding back apostrophes \` themselves).

You can have any prefix or suffix around \`...\`. The whole Selenese parameter is treated as a string and results of Javascript codes from \`...\` are concatenated together with any prefix, suffix and any interlacing strings.

## Compatibility with [ClassicSelenese](ClassicSelenese)
<code>`... $storedVariableName ...`</code> works. However, <code>`...${storedVariableName}...`</code> (as per [ClassicSelenese](ClassicSelenese) > [Stored variables](ClassicSelenese#stored-variables)) doesn't work. Side note: We don't want this combination anyway, because `${storedVariableName}` works through string substitution. It would cause unexpected Javascript errors if `${variableName}` were a number but later it would become a non-numeric string and if there were no quotes/apostrophes around it. Also, it would need extra handling of strings containing apostrophes/quotes. Indeed, `${storedVariableName}` works in prefix/suffix of <code>`...`</code> (as per standard Selenese).

## Passing back apostrophe \` itself
To pass back apostrophe \` itself, double it to \`\`. This applies to any `target` or `value`, whether they contain <code>`...`</code> or not. (Double \` also in prefix or suffix of <code>`...`</code> or in Javascript within <code>`...`</code>). That includes contents of Javascript string literals (within `'...'` or `"..."`) passed to classic `getEval` (and actions generated from it, as per [ClassicSelenese](ClassicSelenese) > [Auto-generated Selenese commands](ClassicSelenese#auto-generated-selenese-commands)). So, with [SelBlocksGlobal](SelBlocksGlobal), any valid Selenese parameter can't contain an odd number of back apostrophes.

# Variations of Javascript within \`...\` with special prefixes
Following are variations of <code>`...`</code> syntax. They all treat the string between the pair of <code>`...`</code> as a Javascript expression. However, they treat the result in a special way, or they apply extra transformation to it.

They all start with special characters `=, \` or `@` immediately in front of the opening backtick \`. Those characters `=, \` and `@` are only a part of the syntax; they are not included in the result that is passed to the Selenese command.

## =\`...\` (with preserved type)
If a Selenese parameter contains only one <code>=`...`</code> with no prefix and no suffix (not even a space), then its result is not treated as a string. [SelBlocksGlobal](SelBlocksGlobal) preserves the type of result of Javascript expression within <code>`...`</code> and it passes the exact result as a Selenese parameter. That's useful if you want to pass a number, an object or an array (and it still works for strings). If there is any prefix or suffix, [SelBlocksGlobal](SelBlocksGlobal) generates an error.

## \\<span></span>\`...\` (a string literal/constant in XPath)
{:#a-string-literalconstant-in-xpath}
<!-- Header ID needed until release of https://github.com/gettalong/kramdown/issues/251 -->
<code>\`...`</code> is like <code>`...`</code>, but it quotes and escapes its result as string literal/constant for XPath expression. You can have multiple occurrences of <code>\`...`</code> in the same Selenese parameter, with any prefix/suffix and interlacing strings, and you can mix them with basic <code>`...`</code>.

Like <code>`...`</code> without prefix, <code>\`...`</code> evaluates the enclosed part as Javascript expression. The result is treated as content of a string literal/constant for XPath. Then this escapes both apostrophes and/or quotation marks in it. It generates an XPath string sub-expression that may use XPath function `concat()` (for details see `quoteForXPath()` in Selenium Core's [htmlutils.js](https://github.com/SeleniumHQ/selenium/blob/master/javascript/selenium-core/scripts/htmlutils.js) or at [_chrome://_ URL](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) _chrome://selenium-ide/content/selenium-core/scripts/htmlutils.js_).

## @\`...\` (an extra Selenese parameter)
By default, Selenium IDE allows to pass only two parameters to commands: `target` (usually a locator) and `value`. However, some SeLite commands (e.g. in [ExitConfirmationChecker](ExitConfirmationChecker)) need to receive extra one or two parameters. [SelBlocksGlobal](SelBlocksGlobal) enhances syntax by allowing value of each standard Selenese parameter (`target` or `value`) to include one occurrence of <code>@`...`</code> containing a Javascript expression.

This notation extracts and completely removes that <code>@`...`</code> from the parameter. It concatenates the rest (any prefix merged with any suffix) for the string value (potentially empty) of that Selenese parameter (`target` or `value`). It transforms that value into a String object (rather than a string primitive). Then it evaluates the Javascript from within <code>@`...`</code> (the part that it extracted and removed). It stores the result as an extra (optional) field `seLiteExtra` on that String object (one made from any prefix and suffix). Then it passes the result String object to the called action as the actual value of the intended Selenese parameter (either `target` or `value`).

The Selenese command (i.e. a custom command or a custom override/intercept of standard Selenese) can access the result of Javascript through field `seLiteExtra` if it is set on the parameter. Using <code>@`...`</code> makes the command receive instance of `String` object rather than a primitive string. That is OK for the most of standard Selenese.

## Special rules on using =\`...\` or @\`...\`

### For script maintainers and framework developers
A simple rule is: Don't pass <code>=`...`</code> or <code>@`...`</code> to `getEval` or to custom commands that evaluate values of their Selenese parameters (`target` or `value`) as Javascript expression(s), unless those commands are designed for it. The same applies to derivative commands like `storeEval` (as per  [ClassicSelenese](ClassicSelenese) > [Auto-generated Selenese commands](ClassicSelenese#auto-generated-selenese-commands)).

Alternatively, if you'd really like to pass the string value of this object as a parameter to command `getEval` (or `storeEval`...), which would then evaluate it as a Javascript expression (again), use <code>`...`</code> instead.

### For developers of custom commands
If you develop Selenese commands that may be used with <code>=`...`</code> or <code>@`...`</code> then:

1. Do not compare the values of standard Selenese parameters (`target` and `value`) using strict comparison operators === and !==. Also, don't depend on their `typeof`, which will be `object` rather than `string` if the passed parameter contains <code>`...`</code> with no prefix and no suffix or if it contains <code>@`...`</code>. If you'd like to use strict comparison or `typeof` with such a parameter, transform it to a string (see the next rule).
2. If the command doesn't need <code>`...`</code> syntax, and the user passes <code>=`...`</code> with no prefix and no suffix, you may want the command to show an error, asking the user to put space(s) as a prefix or suffix, to indicate that the result of <code>=`...`</code> should be cast to a string. See also [selblocks.js](https://code.google.com/p/selite/source/browse/src/chrome/content/extensions/selblocks.js?repo=sel-blocks-global) > `Selenium.prototype.preprocessParameter` and `Selenium.prototype.getEval`.
3. If you implement a Selenese command that evaluates any of its parameters (`target` or `value`) as Javascript and you also want it to work with <code>@`...`</code>, then in the definition of the command
  * get field `seLiteExtra` from the parameter and store it for later use (if needed), e.g. `var seLiteExtra=target.seLiteExtra;`
  * transform the parameter to a primitive string, e.g. `target=''+target;` and only then pass it to `eval()`.

(See also Mozilla's documentation of [String object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) > Distinction between string primitives and String objects.)

# Alternatives in classic Selenese
Selenium IDE supports access to stored variables via `${xyz}` syntax. (Implementation note: it is handled by `preprocessParameter()`.)

Alternatively, you may use shorthand `javascript{...}` - see [Selenium IDE docs](http://docs.seleniumhq.org/docs/02_selenium_ide.jsp) > [JavaScript Usage with Non-Script Parameters](http://docs.seleniumhq.org/docs/02_selenium_ide.jsp#javascript-usage-with-non-script-parameters). However, those Javascript expressions don't support classic `${storedVariableName}` shorthand.

Also, you may call `getEval` as `storeEval`, which stores the evaluated result of `target` parameter in a Selenese stored variable that is named in `value` parameter. E.g. if `value` parameter is `xyz`, then you can access the result as `${xyz}`. However, <code>`...`</code> syntax operates at level different than `getEval`. So, in general you can't just copy any expression between <code>`...`</code> and `getEval` (it works for some expressions, but not for others).

Without <code>`...`</code> you'd need either to

  * create more complex Javascript expressions and pass them through Selenese `javascript{...}`. They'd have to access stored variables via `storedVars`. Such expressions are more wordy and less intuitive.
  * pass the expression(s) through `storeEval` to save the result into a temporary variable. Then pass that stored variable as `${xyz}` to the further command(s). That makes your script longer. The unnecessary variables make it less clear and more fragile.

# Accessing stored variables in Selenese
 * Use `storedVars.variableName` with `getEval` and related commands.
 * Only use `$variableName` with commands from [SelBlocksGlobal](SelBlocksGlobal).
 * Use `${variableName}` or \``$variableName`\` with any other Selenese commands.
 * Keep `javascript{...}` for special purposes. See online [Selenium Core reference](http://release.seleniumhq.org/selenium-core/1.0.1/reference.html) > [Parameter construction and Variables](http://release.seleniumhq.org/selenium-core/1.0.1/reference.html#parameter-construction-and-variables) or offline at [_chrome://_ URL](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) _chrome://selenium-ide/content/selenium-core/reference.html#parameter-construction-and-variables_. Use this notation with e.g. `window.opener.resizeTo()` and `window.opener.innerWidth`.
