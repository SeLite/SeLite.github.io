---
title: SelBlocksGlobal
layout: default
---

# Overview #
SelBlocks Global (one of SeLite [AddOns](AddOns)) is an enhancement of [SelBlocks](https://addons.mozilla.org/en-US/firefox/addon/selenium-ide-sel-blocks/versions/). It enables 

* calling Selenese _functions_ (blocks of steps, formerly called _scripts_) across cases and
* [EnhancedSelenese](EnhancedSelenese)

So cases (within the same suite) can share functions. Since a case can be a part of multiple suites, you may now also reuse functions between suites. The benefits are

  * less maintenance,
  * more compact tests and
  * higher productivity.

See also [AboutDocumentation](AboutDocumentation) > [Terminology](AboutDocumentation#terminology) > [function](AboutDocumentation#function).

SelBlocks Global replaces SelBlocks. Do not use it together with SelBlocks (neither with FlowControl, GoTo nor Sideflow).

# Documentation #
Original documentation of SelBlocks applies to SelBlocks Global (except for incompatibility listed below). See summary of [SelBlocks](https://addons.mozilla.org/en-US/firefox/addon/selenium-ide-sel-blocks/) extension itself and its full [documentation](http://refactoror.wikia.com/wiki/Selblocks_Reference).

See [Selenese reference (online)](https://cdn.rawgit.com/selite/sel-blocks-global/master/src/chrome/content/reference.xml) or (offline) at [_chrome://_ URL](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) _chrome://selite-extension-sequencer/content/selenese_reference.html?chrome://selite-selblocks-global/content/reference.xml_. In addition to those commands, SelBlocks Global also provides [EnhancedSelenese](EnhancedSelenese).

# Differences to SelBlocks #

## Doubling of back apostrophe \`
If parameters of your Selenese actions contain back apostrophe \`, you need to double it into \`\`. See [EnhancedSelenese](EnhancedSelenese).

## Lexical scope ##
When calling a Selenese `function`, it doesn't inherit variables from the higher scope in SelBlocks Global. See [SelBlocks issue #5](https://github.com/refactoror/SelBlocks/issues/5).

## Strict mode ##
SelBlocks Global is in [JavascriptEssential](JavascriptEssential) > [Strict Javascript](JavascriptEssential#strict-javascript), which prevents some bad practice code. That also applies to Javascript expressions passed to SelBlocks Global Selenese commands, or passed through [EnhancedSelenese](EnhancedSelenese) notation \`...\` and #\`...\`. This implies the following incompatibilities with SelBlocks.

### Accessing stored variables ###
When accessing stored variables with `getEval` and special SelBlockGlobal commands, use `$xyz` rather than just `xyz`. SelBlocks Global had to drop shorthand syntax of SelBlocks, which let its special commands access stored variables without using `$` prefix. (That depended on Javascript keyword `with(obj){...}`, which is [prohibited in strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/Strict_mode#Simplifying_variable_uses).) See [EnhancedSelenese](EnhancedSelenese) > [`$storedVariableName` notation](EnhancedSelenese#storedvariablename-notation) for the affected commands.

### Loop `for` ###
`for` loop now must use `$xyz` notation for all stored parameters (loop iterator or any other), whether on the left side or right side of the assignment operator =. So, instead of

```
for|i=1; i<=10; i++
```

use

```
for|$i=1; $i<=10; $i++
```

### Passing parameters to functions via `call` ###
`call` can use `$xyz`, but only in expressions on the right side of parameter assignments `parameterName=expression`. The formal parameter names on the left (ones being passed to the function) must not start with $. So, instead of

```
call|myFunction|myParam=storedVariableInCallingScope
```

use

```
call|myFunction|myParam=$storedVariableInCallingScope
```

## Try/catch suppresses error counts ##
`try...catch` suppresses error counts and some error logs for exceptions, errors or failures of asserts/verifications. This benefits scripts that test Selenese commands themselves (e.g. ones provided by SeLite or any custom commands).

# Flow control with Selenese boolean accessors
Selenium, SeLite and custom add-ons define `isXyz()` Selenese boolean accessors. You may combine them with `if`, `elseIf` or `while`, by passing `selenium.isXyz()` or `selenium.isXyz('locatorString')`. Indeed, you may combine the accessor calls in more complex boolean expressions. For example:

```
if | !selenium.isVisible( 'id=pmf-navbar-collapse' )
```

@TODO DOC Selenese > ???: CombiningExpressions: Use variable `selenium` in Selenium Core scope for the same as what `this` keyword is in context of Selenese actions `getEval` (and related), if, while. However, in their context `this===selenium`, hence use `selenium` instead of `this`, so that it's the same as in Selenium Core scope. (In other scopes, e.g. in a Selenium IDE extension or a [Javascript code module](JavascriptComplex#javascript-code-modules), load `SeLiteMisc` code module and use `SeLiteMisc.selenium` instead).
-> So, unless you need a result of a Javascript expression in multiple places in the same Selenese part or the same Selenese function, don't call `storeEval` but use [EnhancedSelenese](EnhancedSelenese) to pass the Javascript expression directly within paired backticks \`...\` (or =\`...\`, if the result can be other than a string). This minimises a need to use `storeEval` and auxiliary stored variables. That makes Selenese scripts shorter and clearer.