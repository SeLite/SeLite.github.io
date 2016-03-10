---
layout: default
---
{% include links %}

# Overview #
SelBlocks Global (one of SeLite [AddOns](AddOns)) is an enhancement of [SelBlocks](https://github.com/refactoror/SelBlocks/). It enables 

* calling Selenese _functions_ (blocks of steps, formerly called _scripts_) across cases and
* [EnhancedSelenese](EnhancedSelenese)

So cases (within the same suite) can share functions. Since a case can be a part of multiple suites, you may now also reuse functions between suites. The benefits are

  * less maintenance,
  * more compact [scripts][script] and
  * higher productivity.

See also [AboutDocumentation](AboutDocumentation) > [Terminology](AboutDocumentation#terminology) > [function](AboutDocumentation#function).

SelBlocks Global replaces SelBlocks. Do not use it together with SelBlocks (neither with FlowControl, GoTo nor Sideflow).

# Documentation #
Original documentation of SelBlocks applies to SelBlocks Global (except for incompatibility listed below). See summary of [SelBlocks](https://addons.mozilla.org/en-US/firefox/addon/selenium-ide-sel-blocks/) extension itself and its full [documentation](http://refactoror.wikia.com/wiki/Selblocks_Reference).

See [Selenese reference (online)](https://cdn.rawgit.com/SeLite/SelBlocksGlobal/master/sel-blocks-fx_xpi/chrome/content/reference.xml) or (offline) at {{chromeUrl}} _chrome://selite-extension-sequencer/content/selenese_reference.html?chrome://selite-selblocks-global/content/reference.xml_. In addition to those commands, SelBlocks Global also provides [EnhancedSelenese](EnhancedSelenese).

# Differences to SelBlocks #

## Usage of &lt;&gt;
See [EnhancedSelenese](EnhancedSelenese).

## Lexical scope ##
When calling a Selenese `function`, it doesn't inherit variables from the higher scope in SelBlocks Global. See [SelBlocks issue #5](https://github.com/refactoror/SelBlocks/issues/5).

## Strict mode ##
SelBlocks Global adheres to {{navStrictJavascript}}, which prevents some bad practice code. That also applies to Javascript expressions passed to SelBlocks Global Selenese commands, or passed through [EnhancedSelenese](EnhancedSelenese) notation `<>...<>` (and its variations). This implies the following incompatibilities with SelBlocks.

### Accessing stored variables ###
When accessing stored variables with `getEval` and special SelBlockGlobal commands, use `$xyz` rather than just `xyz`. SelBlocks Global had to drop shorthand syntax of SelBlocks, which let its special commands access stored variables without using `$` prefix. (That depended on Javascript keyword `with(obj){...}`, which is [prohibited in strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/Strict_mode#Simplifying_variable_uses).) See [EnhancedSelenese](EnhancedSelenese) > [`$storedVariableName` notation](EnhancedSelenese#storedvariablename-notation) for the affected commands.

Apply the same to right side of assignments in `value` parameter of `call`. See [Passing parameters to functions via `call`](#passing-parameters-to-functions-via-call).

### Loop `for` ###
`for` loop now must use `$xyz` notation for all stored parameters (loop iterator or any other), whether on the left side or right side of the assignment operator =. So, instead of

```
for | i=1; i<=10; i++
```

use

```
for | $i=1; $i<=10; $i++
```

## Passing parameters to functions via `call` ##

### Modified classic way ###
This is similar to SelBlocks, but modified due to [Strict mode](#strict-mode). `call` has to prefix `$` when accessing stored variables in expressions on the right side of parameter assignments `parameterName=expression`. Hence, instead of

```
call | myFunction | myParam=storedVariableInCallingScope
```

use

```
call | myFunction | myParam=$storedVariableInCallingScope
```

### Passing an object ###
This benefits from [EnhancedSelenese](EnhancedSelenese) > [`=<>…<> (with preserved type)`](EnhancedSelenese#with-preserved-type). Pass an object in Selenese `value` parameter. Its direct fields become parameters passed to the chosen Selenese function (defined by respective Selenese block `function...endFunction`).

Use either an existing object, or an object literal. E.g.

```
call | myFunction | =<>{seleneseParamName1: value1, seleneseParamName2: value2...}<>
```

## 'Synchronous' Selenese calls
A Selenese command (usually `getEval` or a custom command) can invoke Javascript. You may want to trigger other Selenese commands from that Javascript. Use `selenium.callBack( nameOfseleneseFunction, seleneseFunctionParameters )`. Don't invoke it from asynchronous handlers.

That injects a call to given Selenese function after the current Selenese command (i.e. `getEval` or a custom command). Handle any failures with `try...catch...finally...endTry`.

## Asynchronous Selenese calls
Use `selenium.callFromAsync( nameOfseleneseFunction, seleneseFunctionParameters, [onSuccess, [onFailure]] )`.

See Preview.

This requires Selenium to finish any running.

## Try/catch suppresses error counts ##
`try...catch` suppresses error counts and some error logs for exceptions, errors or failures of asserts/verifications. This benefits [scripts][script] that verify functionality of custom [commands][command].

# Flow control with Selenese boolean accessors
Selenium, SeLite and custom add-ons define `isXyz()` Selenese boolean accessors. You may combine them with `if`, `elseIf` or `while`, by passing `selenium.isXyz()` or `selenium.isXyz('locatorString')`. Indeed, you may combine the accessor calls in more complex boolean expressions. For example:

```
if | !selenium.isVisible( 'id=pmf-navbar-collapse' )
```

@TODO > include links
@TODO DOC Selenese > ???: CombiningExpressions: Use variable `selenium` in [Core scope] for the same as what `this` keyword is in context of Selenese actions `getEval` (and related), if, while. However, in their context `this===selenium`, hence use `selenium` instead of `this`, so that it's the same as in [Core scope]. (In other scopes, e.g. in an [IDE extension] or a [Javascript code module](JavascriptComplex#javascript-code-modules), load `SeLiteMisc` code module and use `SeLiteMisc.selenium` instead<!--TODO example of loading-->.)
-> So, unless you need a result of a Javascript expression in multiple places in the same Selenese part or the same Selenese function, don't call `storeEval` but use [EnhancedSelenese](EnhancedSelenese) to pass the Javascript expression directly within pairs `<>...<>` (or `=<>...<>` if the result can be other than a string). This minimises use of `storeEval` and auxiliary stored variables. That makes Selenese scripts shorter, clearer, more robust and reusable.