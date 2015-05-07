---
title: Enhanced Selenese syntax
layout: default
---
[SelBlocksGlobal](SelBlocksGlobal) provides an enhancement to syntax of parameters _target_ and _value_ passed to Selenese commands. It allows those expressions to conveniently access stored variables through _$storedVariableName_ notation. That makes tests shorter and clearer.

# Javascript within \`...\` without special prefix (cast to a string) #
This notation allows you to pass results of one or multiple Javascript expressions (each enclosed within a pair of back ticks \`...\`) to Selenese commands in their parameter (_target_ or _value_). It evaluates any Javascript code in _target_ or _value_ that is between a pair of \`...\`. Then it converts the result to a string (excluding back apostrophes \` themselves).

You can have any prefix or suffix around \`...\`. The whole Selenese parameter is treated as a string and results of Javascript codes from \`...\` are concatenated together with any prefix, suffix and any interlacing strings.

_\`... $storedVariableName ...\`_ works. However, _\`...${storedVariableName}...\`_ doesn't work. Side note: We don't want this combination anyway, because _${storedVariableName}_ works through string substitution. It would cause unexpected Javascript errors if _${variableName}_ were a number but later it would become a non-numeric string and if there were no quotes/apostrophes around it. Also, it would need extra handling of strings containing apostrophes/quotes. Indeed, _${storedVariableName}_ works in prefix/suffix of _\`...\`_ (as per standard Selenese).

## Passing back apostrophe \` itself
To pass back apostrophe \` itself, double it to \`\`. This applies to any _target_ or _value_, whether they contain \`...\` or not. (Double \` also in prefix or suffix of \`...\` or in Javascript within \`...\`). That includes contents of Javascript string literals (within '...' or "...") passed to classic _getEval_ (and actions generated from it, as per [SeleniumIDE](SeleniumIDE) > [Auto-generated Selenese commands](SeleniumIDE#auto-generated-selenese-commands)). So, with [SelBlocksGlobal](SelBlocksGlobal), any valid Selenese parameter can't contain an odd number of back apostrophes.

# Variations of Javascript within \`...\` with special prefixes
Following are variations of \`...\` syntax. They all treat the string between the pair of \`...\` as a Javascript expression. However, they treat the result in a special way, or they apply extra transformation to it.

They all start with special characters =, \ or @ immediately in front of the opening backtick \`. Those characters =, \ and @ are only a part of the syntax; they are not included in the result that is passed to the Selenese command.

## =\`...\` (with preserved type)
If a Selenese parameter contains only one =\`...\` with no prefix and no suffix (not even a space), then its result is not treated as a string. [SelBlocksGlobal](SelBlocksGlobal) preserves the type of result of Javascript expression within =\`...\` and it passes the exact result as a Selenese parameter. That's useful if you want to pass a number, an object or an array (and it still works for strings). If there is any prefix or suffix, [SelBlocksGlobal](SelBlocksGlobal) generates an error.

## \\\`...\` (a string literal/constant in XPath)
\\\`...\` is like \`...\`, but it quotes and escapes its result as string literal/constant for XPath expression. You can have multiple occurrences of \\\`...\` in the same Selenese parameter, with any prefix/suffix and interlacing strings, and you can mix them with basic \`...\`.

\\\`...\` evaluates the javascript expression. The result is treated as content of a string literal/constant for XPath. Then this escapes both apostrophes and/or quotation marks in it. It generates an XPath string sub-expression that may use XPath function _concat()_ (for details see _quoteForXPath()_ in Selenium Core's [htmlutils.js](https://github.com/SeleniumHQ/selenium/blob/master/javascript/selenium-core/scripts/htmlutils.js) or at [_chrome://_ URL](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) _file:///home/pkehl/selenium-ide-2.9.0/chrome/content/selenium-core/scripts/htmlutils.js_).

## @\`...\` (an extra Selenese parameter)
By default, Selenium IDE allows to pass only two parameters to commands: _target_ (usually a locator) and _value_. However, some SeLite commands (e.g. in [ExitConfirmationChecker](ExitConfirmationChecker)) need to receive extra one or two parameters. [SelBlocksGlobal](SelBlocksGlobal) enhances syntax by allowing value of each standard Selenese parameter (_target_ or _value_) to include one occurrence of _@\`...\`_ containing a Javascript expression.

This notation extracts and completely removes that _@\`...\`_ from the parameter. It concatenates the rest (any prefix merged with any suffix) for the string value (potentially empty) of that Selenese parameter (_target_ or _value_). It transforms that value into a String object (rather than a string primitive). Then it evaluates the Javascript from within _@\`...\`_ (the part that it extracted and removed). It stores the result as an extra (optional) field _seLiteExtra_ on that String object (one made from any prefix and suffix). Then it passes the result String object to the called action as the actual value of the intended Selenese parameter (either _target_ or _value_).

The Selenese command (i.e. a custom command or a custom override/intercept of standard Selenese) can access the result of Javascript through field _seLiteExtra_ if it is set on the parameter. Using _@\`...\`_ makes the command receive instance of _String_ object rather than a primitive string. That is OK for the most of standard Selenese.

## Special rules on using =\`...\` or @\`...\`

### For script maintainers and framework developers
A simple rule is: Don't pass _=\`...\`_ or _@\`...\`_ to _getEval_ or to custom commands that evaluate values of their Selenese parameters (_target_ or _value_) as Javascript expression(s), unless those commands are designed for it. The same applies to derivative commands like _storeEval_ (as per  [SeleniumIDE](SeleniumIDE) > [Auto-generated Selenese commands](SeleniumIDE#auto-generated-selenese-commands)).

Alternatively, if you'd really like to pass the string value of this object as a parameter to command _getEval_ (or _storeEval_...), which would then evaluate it as a Javascript expression (again), use \`...\` instead.

### For developers of custom commands
If you develop Selenese commands that may be used with _=\`...\`_ or _@\`...\`_ then:

1. Do not compare the values of standard Selenese parameters (_target_ and _value_) using strict comparison operators === and !==. Also, don't depend on their _typeof_, which will be 'object' rather than 'string' if the passed parameter contains =\`...\` with no prefix and no suffix or if it contains @\`...\`. If you'd like to use strict comparison or _typeof_ with such a parameter, transform it to a string (see the next rule).
2. If the command doesn't need @\`...\` syntax, and the user passes =\`...\` with no prefix and no suffix, you may want the command to show an error, asking the user to put space(s) as a prefix or suffix, to indicate that the result of =\`...\` should be cast to a string. See also [selblocks.js](https://code.google.com/p/selite/source/browse/src/chrome/content/extensions/selblocks.js?repo=sel-blocks-global) > _Selenium.prototype.preprocessParameter_ and _Selenium.prototype.getEval_.
3. If you implement a Selenese command that evaluates any of its parameters (_target_ or _value_) as Javascript and you also want it to work with @\`...\`, then in the definition of the command
  * get field _seLiteExtra_ from the parameter and store it for later use (if needed), e.g. <em>var seLiteExtra=target.seLiteExtra;</em>
  * transform the parameter to a primitive string, e.g. <em>target=''+target;</em> and only then pass it to _eval()_.
(See also Mozilla's documentation of [String object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) > Distinction between string primitives and String objects.)

# Alternatives in classic Selenese
Selenium IDE supports access to stored variables via _${xyz}_ syntax. (Implementation note: it is handled by _preprocessParameter()_.)

Alternatively, you may use shorthand _javascript{...}_ - see [Selenium IDE docs](http://docs.seleniumhq.org/docs/02_selenium_ide.jsp) > [JavaScript Usage with Non-Script Parameters](http://docs.seleniumhq.org/docs/02_selenium_ide.jsp#javascript-usage-with-non-script-parameters). However, those Javascript expressions don't support classic _${storedVariableName}_ shorthand.

Also, you may call _getEval_ as _storeEval_, which stores the evaluated result of _target_ parameter in a Selenese stored variable that is named in _value_ parameter. E.g. if _value_ parameter is _xyz_, then you can access the result as _${xyz}_. However, \`...\` syntax operates at level different than _getEval_. So, in general you can't just copy any expression between \`...\` and _getEval_ (it works for some expressions, but not for others).

Without \`...\` you'd need either to

  * create more complex Javascript expressions and pass them through Selenese _javascript{...}_. They'd have to access stored variables via _storedVars_. Such expressions are more wordy and less intuitive.
  * pass the expression(s) through _storeEval_ to save the result into a temporary variable. Then pass that stored variable as _${xyz}_ to the further command(s). That makes your script longer. The unnecessary variables make it less clear and more fragile.

# Accessing stored variables in Selenese
 * Use _storedVars.variableName_ with _getEval_ and related commands.
 * Only use _$variableName_ with commands from [SelBlocksGlobal](SelBlocksGlobal).
 * Use _${variableName}_ or _\`$variableName\`_ with any other Selenese commands.
 * Keep _javascript{...}_ for special purposes. See [Selenium Core reference](http://release.seleniumhq.org/selenium-core/1.0.1/reference.html) > [Parameter construction and Variables](http://release.seleniumhq.org/selenium-core/1.0.1/reference.html#parameter-construction-and-variables) or [_chrome://_ URL](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) _chrome://selenium-ide/content/selenium-core/reference.html#parameter-construction-and-variables_. Use this notation with e.g. _window.opener.resizeTo()_ and _window.opener.innerWidth_.
