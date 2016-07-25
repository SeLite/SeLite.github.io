---
layout: default
---
{% include links %}
* TOC
{:toc}

# Overview #
SelBlocks Global ([Components](Components) > [SelBlocks Global](Components#selblocksglobal)) is an enhancement of [SelBlocks](https://github.com/refactoror/SelBlocks/). It enables 

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
When accessing stored variables with `getEval` and SelBlockGlobal commands that evaluate Javascript (e.g.`if, while, promise...`), use `$xyz` rather than just `xyz`. SelBlocks Global had to drop shorthand syntax of SelBlocks, which let its special commands access stored variables without using `$` prefix. (That depended on Javascript keyword `with(obj){...}`, which is [prohibited in strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/Strict_mode#Simplifying_variable_uses).) See [EnhancedSelenese](EnhancedSelenese) > [`$storedVariableName` notation](EnhancedSelenese#storedvariablename-notation) for the affected commands.

Apply the same to right side of assignments in `value` parameter of `call`. See [Passing parameters to functions via `call`](#passing-parameters-to-functions-via-call).

Also use `$storedVariableName` in [EnhancedSelenese](EnhancedSelenese) > [Javascript within &lt;&gt;...&lt;&gt;](EnhancedSelenese#javascript-within).

### Loop `for` ###
`for` loop now must use `$xyz` notation for all stored parameters (loop iterator or any other), whether on the left side or right side of the assignment operator =. So, instead of

```
for | i=1; i<=10; i++
```

use

```
for | $i=1; $i<=10; $i++
```

### Removed `isOneOf(), mapTo(), translate()`
SelBlocks added functions `isOneOf(), mapTo()` and `translate()` to `String.prototype`. However, Mozilla rules don't allow extensons of built-in JS types anymore. That's for security and compatibility reasons. Hence SelBlocks Global had to removed them.

try...endTry can catch timeout

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
call | myFunction | =<>({ seleneseParamName1: value1, seleneseParamName2: value2... })<>
```

## `storeEval` can return null/undefined
`storeEval` in classic Selenium IDE couldn't store `null` or `undefined`. Instead, it replaced them with string `"null"`. In SelBlocks Global it stores `null` and `undefined` unchanged.

## Renamed `foreach` to `forEach`
This renamed `foreach` to `forEach` (and `endForeach` to `endForEach`). The old commands still work for now. 

# New structures
These are forward-compatible with classic SelBlocks.

## Usage of &lt;&gt;
See [EnhancedSelenese](EnhancedSelenese).

## Promise-based commands
These commands **wait** for a given `Promise` object to resolve. If it gets rejected or it times out, then the command throws an error.

 * `promise` is similar to `getEval`.
 * `storePromiseValue` is similar to `storeEval`, but it stores the resolved (fulfilled) value of the promise (rather than the promise itself).
 * `ifPromise...elseIfPromise...elsePromise...endPromise` is similar to `if...elseIf...else...endIf`. `whilePromise...endWhilePromise` is similar to `while...endWhile`. `repeatPromise...untilPromise` is similar to `repat...until`. However, these commands wait for given `Promise` object to resolve (fulfill). If the promise resolves to non-strict false (i.e. any of `false, null, 0, ` empty string `""` or `undefined`), then it runs appropriate `elseIfPromise` or `endPromise` branch, or the loop ends. If the promise gets rejected or it times out, the command throws a (catcheable) error.

You may want to increase `selenium.defaultTimeout` programatically (through `getEval`). Such a change applies only to the current [case] run. That covers any [functions][function] from other [cases][case], but only while running the top [case] that modified `selenium.defaultTimeout`. It gets reset before running any further cases from the same suite (or before running the rest of favorites, if using [Components](Components) > [Run All Favorites](Components#run-all-favorites)).

## `repeat...until` loop (with a condition at the end)
`repeat...until` is a loop with a condition at the end. It runs its body at least once. Its condition indicates its end: the loop repeats the body until the condition evaluates to non-strict `true` (i.e. not `false, null, 0, ` empty string `""` neither `undefined`).

`repeatPromise...untilPromise` is similar. However, the condition must evaluate to a `Promise`. The loop repeats until the promise resolves to `true`. If the promise rejects or times out, this throws a (catcheable) error.

## Iterator and iterable-based loops
`forIterator...endForIterator` and `forIterable...endForIterable` iterate the given iterator or iterable object. See [MDN > Iteration protocols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols).

## Javascript callbacks to Selenium
These invoke a Selenese function from user's Javascript. The Selenese function executes in Selenium IDE as if it were invoked through `call` command during a standard case/suite run. The user can pause it, inspect stored variables and logs.

### In-flow Selenese callbacks
A Selenese command (usually `getEval` or a custom command) can invoke Javascript. From there you may want to trigger other Selenese commands. Use `selenium.callBackInFlow( nameOfSeleneseFunction, seleneseFunctionParameters )`. (Don't invoke it from the last command of any test case). This is re-entrant (it can be multi-level). However, don't invoke it from asynchronous handlers.

That injects a call to given Selenese function, but only after the current Selenese command (i.e. `getEval` or a custom command) finishes. Handle any failures with `try...catch...finally...endTry` outside the current Selenese command (which triggered `selenium.callBackInFlow(...)`).

Be careful when implementing a workflow that uses `selenium.callBackInFlow(...)`. This Javascript code doesn't invoke the Selenese function immediately. It only adds it to Selenium IDE schedule, and it returns the control back. Therefore any following Javascript code must not depend or affect the running of that Selenese function. As a rule of thumb, trigger such a call at the latest possible point in your Javascript.

### Out-of-flow Selenese callbacks
These invoke a Selenese function 'asynchronously' when your Selenese script is not running. That allows Selenese scripts to run in stages. Use `selenium.callBackOutFlow( nameOfSeleneseFunction, seleneseFunctionParameters )`. This returns [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise). Use `then( function successHandler(value) )` or `then( function successHandler(value), function failureHandler(exception) )` to handle callback outcome.

These callbacks are especially useful for presenting with [Preview](Preview).

## Try/catch suppresses error and timeout counts ##
`try...catch` suppresses counts and some logs for errors, failures of asserts/verifications and timeouts. [Scripts][script] can verify that custom [commands][command] fail or time out as expected (and if they fail or time out, the script succeeds; on the other hand, if the command succeeds, the test script fails).

# Functions and commands

## `Selenium.download()`
If you use Windows, see [ThirdPartyIssues](ThirdPartyIssues) > [Backslashes get reduced to half](https://github.com/SeleniumHQ/selenium/issues/2215).

## `browserbot.getElements()`
In [core] you can use `this.browserbot.getElements()` to get an array of matching elements (if any).

## `breakPoint` command

# Productivity tip

## Selenese accessors in Javascript expressions
You may combine `getEval` (and its derivatives), `promise`, `if`, `while` and other commands that evaluate Javascript, with Selenese accessors. For example:

```
if | !selenium.isVisible( 'id=pmf-navbar-collapse' )
storeEval | selenium.browserbot.findElement('locatorString').innerText | storedVariableName
getEval | more-complex-expression... selenium.getAttribute('locatorString@attributeName')...
```

 Use variable `selenium` instead of `this`. Those two are the same in [Core scope]. However, `selenium` is more expressive. (Also `this` has a different meaning in closures.) For example: `if | selenium.isElementPresent( 'locator...' )| ... endIf`.

In other scopes, e.g. in an [IDE extension] or a [Javascript code module](JavascriptComplex#javascript-code-modules), load `SeLiteMisc` code module `Components.utils.import( 'chrome://selite-misc/content/SeLiteMisc.js' );` and use `SeLiteMisc.selenium` instead.