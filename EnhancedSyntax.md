---
title: Enhanced Selenese syntax
layout: default
---
[SelBlocksGlobal](SelBlocksGlobal) provides an enhancement to syntax of parameters _target_ and _value_ passed to Selenese commands. It allows those expressions to conveniently access stored variables through $ notation (any _$xxx_ refers to stored variable _xxx_). That allows the tests to be shorter and possibly clearer.

# Javascript within \`...\` without special prefix (cast to a string) #
This notation allows you to pass results of one or multiple Javascript expressions (each enclosed within a pair of back ticks \`...\`) to Selenese commands in their parameter (Target or Value). This syntax is an alternative to both Selenese standard _javascript{...}_ and SelBlocks's simplified syntax _$xyz_.

This evaluates any Javascript code in Target or Value that is between a pair of \`...\`. It then converts the result to a string (excluding back apostrophes \` themselves).

You can have any prefix or postfix around \`...\`. The whole Selenese parameter is treated as a string and results of Javascript codes from \`...\` are concatenated together with any prefix, postfix and any interlacing strings.

\`... $nameOfStoredVariable ...\` works. However, \`...${...}...\` doesn't work. Side note: We don't want it anyway, because ${...} works through substitution. It would cause unexpected errors if ${variableName} were a number and later it would become a non-numeric string and if there were no quotes/apostrophes around it. Also, it would need extra handling of strings containing apostrophes/quotes. Indeed, ${...} works in prefix/postfix of \`...\`.

If you want to pass back apostrophe \` itself, double it to \`\`. That applies to any Target or Value, whether they contain \`...\` or not; and if yes, then you can pass \`\` in prefix or postfix of \`...\` or within \`...\`. That includes contents of Javascript string literals (within '...' or "...") passed to classic _getEval_ (and actions generated from it, as per [SeleniumIDE](SeleniumIDE) > [Auto-generated Selenese commands](SeleniumIDE#auto-generated-selenese-commands). So, with [SelBlocksGlobal](SelBlocksGlobal), any valid Selenese parameter can't contain an odd number of back apostrophes.

# Variations of Javascript within \`...\` with special prefixes
Following are variations of \`...\` syntax. They all treat the string between the pair of \`...\` as a Javascript expression. However, they treat the result in a special way, or they apply extra transformation to it.

They all start with special characters =, \ or @ immediately in front of the opening backtick \`. Those characters =, \ and @ are only a part of the syntax; they are not included in the result that is passed to the Selenese command.

## =\`...\` (with preserved type)
If a Selenese parameter contains only one =\`...\` with no prefix and no postfix (not even a space), then its result is not treated as a string. [SelBlocksGlobal](SelBlocksGlobal) preserves the type of result of Javascript expression within =\`...\` and it passes the exact result as a Selenese parameter. That's useful if you want to pass a number, an object or an array (and it still works for strings). If there is any prefix or postfix, [SelBlocksGlobal](SelBlocksGlobal) generates an error.

## \\\`...\` (a string literal/constant in XPath)
\\\`...\` is like \`...\`, but it quotes and escapes its result as string literal/constant for XPath expression. You can have multiple occurrences of \\\`...\` in the same Selenese parameter, with any prefix/postfix and interlacing strings, and you can mix them with basic \`...\`.

\\\`...\` evaluates the javascript expression. The result is treated as content of a string literal/constant for XPath. This then escapes both apostrophes and/or quotation marks in it. It generates an XPath string sub-expression that may use XPath function _concat()_ (for details see _quoteForXPath()_ in Selenium Core's [htmlutils.js](https://github.com/SeleniumHQ/selenium/blob/master/javascript/selenium-core/scripts/htmlutils.js) or at [_chrome://_ URL](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) _file:///home/pkehl/selenium-ide-2.9.0/chrome/content/selenium-core/scripts/htmlutils.js_).

## @\`...\` (an extra Selenese parameter)
By default, Selenium IDE allows to pass only two parameters to commands: Target (usually a locator) and Value. However, some SeLite commands (e.g. in [ExitConfirmationChecker](ExitConfirmationChecker)) need to receive extra one or two parameters. [SelBlocksGlobal](SelBlocksGlobal) enhances syntax by allowing value of each standard Selenese parameter (_Target_ or _Value_) to include one occurrence of @\`...\` containing a Javascript expression. It extracts and completely removes that @\`...\` from the parameter; it passes the rest (prefix merged with postfix) for that classic parameter (_Target_ or _Value_). It evaluates the Javascript (from the part that it extracted and removed) and it stores the result as an extra (optional) field _seLiteExtra_ on that String object (one made from any prefix and postfix).
Then it passes the result String object to the called action as a value of the intended Selenese parameter (_target_ or _value_).

The Selenese command (i.e. a custom command or a custom override/intercept of standard Selenese) can access the result of Javascript through field _seLiteExtra_ if it is set on the parameter. Using @\`...\` makes the command receive instance of _String_ object rather than a primitive string. That is OK for the most of standard Selenese.

## Special rules on using =\`...\` or @\`...\`
### For script maintainers and framework developers
A simple rule is: Don't pass =\`...\` or @\`...\` to _getEval_ or to custom commands that evaluate values of their Selenese parameters (Target or Value) as Javascript expression(s), unless those commands are designed for it. The same applies to derivative commands like _storeEval_ (as per  [SeleniumIDE](SeleniumIDE) > [Auto-generated Selenese commands](SeleniumIDE#auto-generated-selenese-commands)).

Alternatively, if you'd really like to pass the string value of this object as a parameter to command _getEval_ (or _storeEval_...), which would then evaluate it as a Javascript expression (again), use \`...\` instead.

### For developers of custom commands
If you develop Selenese commands that may be used with =\`...\` or @\`...\` then:

1. Do not compare the values of standard Selenese parameters (_target_ and _value_) using strict comparison operators === and !==. Also, don't depend on their _typeof_, which will be 'object' rather than 'string' if the passed parameter contains =\`...\` with no prefix and no postfix or if it contains @\`...\`. If you'd like to use strict comparison or _typeof_ with such a parameter, transform it to a string (see the next rule).
2. If the command doesn't need @\`...\` syntax, and the user passes =\`...\` with no prefix and no postfix, you may want the command to show an error, asking the user to put space(s) as a prefix or postfix, to indicate that the result of =\`...\` should be cast to a string. See also [selblocks.js](https://code.google.com/p/selite/source/browse/src/chrome/content/extensions/selblocks.js?repo=sel-blocks-global) > _Selenium.prototype.preprocessParameter_ and _Selenium.prototype.getEval_.
3. If you implement a Selenese command that evaluates any of its parameters (_target_ or _value_) as Javascript and you also want it to work with @\`...\`, then in the definition of the command
  * get field _seLiteExtra_ from the parameter and store it for later use (if needed), e.g. <i>var seLiteExtra=target.seLiteExtra;</i>
  * transform the parameter to a primitive string, e.g. <i>target=''+target;</i> and only then pass it to _eval()_.
(See also Mozilla's documentation of [String object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) > Distinction between string primitives and String objects.)

# Alternatives in classic Selenese
Selenium IDE supports access to stored variables via _${xyz}_ syntax. (Implementation note: it is handled by _preprocessParameter()_.)

Alternatively, you may use shorthand _javascript{...}_ - see [Selenium IDE docs](http://docs.seleniumhq.org/docs/02_selenium_ide.jsp) > [JavaScript Usage with Script Parameters](http://docs.seleniumhq.org/docs/02_selenium_ide.jsp#javascript-and-selenese-parameters). However, Javascript expressions passed within _javascript{...}_ don't support classic shorthand to standard stored variables _${xyz}_.

Also, you may call _getEval_ as _storeEval_, which stores the evaluated result of Target parameter in a Selenese variable that is named in Value parameter. E.g. if Value parameter is _xyz_, then you can access the result as _${xyz}_. However, \`...\` syntax operates at level different to level of _getEval_. So, you can't just copy any expression between \`...\` and _getEval_ (it would work for some values, but not for others).

Without \`...\` you'd need either to
  * create more complex Javascript expressions and pass them through Selenese _javascript{...}_. They'd have to access stored variables via _storedVars_. Such expressions are more wordy and less intuitive.
  * pass the expression(s) through _storeEval_ to save the result into a temporary variable. Then pass that stored variable as _${xyz}_ to the further command(s). That makes your script longer. The unnecessary variables make it less clear and more fragile.
