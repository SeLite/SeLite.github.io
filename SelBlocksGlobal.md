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

A known limitation: After you pause a run of Selenese script, don't run any single command. Otherwise resuming the script afterwards fails.

# Changes from SelBlocks #
Following changes are not backwards compatible with classic SelBlocks. However, they are trivial and intuitive.

## Lexical scope ##
When calling a Selenese `function`, it doesn't inherit variables from the higher scope in SelBlocks Global. See [SelBlocks issue #5](https://github.com/refactoror/SelBlocks/issues/5).

`for... endFor` loop keeps the iterator variable(s) after it finishes. (Hence, these override any variable(s) with the same name(s) that existed before the loop. Classic SelBlocks protects those outer variables, but it removes the iterators from the scope afterwards.) That's useful especially when the loop finishes by `break`.

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
This benefits from [EnhancedSelenese](EnhancedSelenese) > [`=<>…<> (with preserved type)`](EnhancedSelenese#with-preserved-type). Pass an object in Selenese `value` parameter. Its direct fields become parameters passed to the chosen Selenese function (defined by respective Selenese block `function...endFunction`). (This also enables expressions that contain a comma, which is not allowed by syntax of classic SelBlocks.)

Use either an existing object, or an object literal. E.g.

```
call | myFunction | =<>{seleneseParamName1: value1, seleneseParamName2: value2...}<>
```

# Enhancements to SelBlocks #
These are forward-compatible with classic SelBlocks.

## Usage of &lt;&gt;
See [EnhancedSelenese](EnhancedSelenese).

## Call backs
These invoke Selenese functions programatically. Custom Javascript can trigger Selenese functions, which then run in Selenium IDE. The user can pause them, inspect stored variables and logs. The biggest benefit is in presenting with [Preview](Preview).

### 'Synchronous' Selenese calls
A Selenese command (usually `getEval` or a custom command) can invoke Javascript. From there you may want to trigger other Selenese commands. Use `selenium.callBack( nameOfseleneseFunction, seleneseFunctionParameters )`. (Don't invoke it from the last command of any test case). Don't invoke it from asynchronous handlers.

That injects a call to given Selenese function, but only after the current Selenese command (i.e. `getEval` or a custom command) finishes. Handle any failures with `try...catch...finally...endTry` outside the current Selenese command (which triggered `selenium.callBack(...)`).

Be careful when implementing a workflow that uses `selenium.callBack(...)`. This Javascript code doesn't invoke the Selenese function immediately. It only adds it to Selenium IDE schedule, and it returns the control back. Therefore any following Javascript code must not depend or affect the running of that Selenese function. As a rule of thumb, trigger such a call at the latest possible point in custom Javascript.

### Asynchronous Selenese calls
You can invoke Selenese functions 'asynchronously' after your Selenese script finished its run. That allows Selenese scripts to run in stages. Use `selenium.callFromAsync( nameOfseleneseFunction, seleneseFunctionParameters, [onSuccess, [onFailure]] )`.

See [Preview](Preview).

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