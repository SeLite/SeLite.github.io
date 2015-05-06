---
title: SelBlocksGlobal
layout: default
---

# Overview #
SelBlocks Global is one of SeLite AddOns. It's an enhancement of [SelBlocks](https://addons.mozilla.org/en-US/firefox/addon/selenium-ide-sel-blocks/versions/). SelBlocks allows reusing blocks of steps grouped into functions (formerly called 'scripts'). However, it only lets you call the functions from within the same test case.

If we could call functions across test cases, we could organise them better than in one long file. A test case can be a part of multiple test suites. So we could share functions between test suites, too. That means less copy and paste code. If you structure your functions well, you'll have

  * less maintenance
  * more compact tests
  * higher productivity.

SelBlocks Global allows you to do that. It can call functions from other test cases (within the same test suite). Since a test suite can contain any test cases (from anywhere on the filesystem), you can share test cases with functions between any test suites. See also [AboutDocumentation](AboutDocumentation) > [Terminology](AboutDocumentation#terminology) > [function](AboutDocumentation#function).

It replaces SelBlocks. You can't use SelBlocks Global together with SelBlocks (neither with FlowControl, GoTo nor Sideflow).

# Documentation #
Original documentation of SelBlocks applies to SelBlocks Global (except for incompatibility listed below). See summary of [SelBlocks](https://addons.mozilla.org/en-US/firefox/addon/selenium-ide-sel-blocks/) extension itself and its full [documentation](http://refactoror.wikia.com/wiki/Selblocks_Reference).

See [Selenese reference (online)](https://cdn.rawgit.com/selite/sel-blocks-global/master/src/chrome/content/reference.xml) or (offline) at [_chrome://_ URL](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) _chrome://selite-extension-sequencer/content/selenese_reference.html?chrome://selite-selblocks-global/content/reference.xml_. In addition to those commands, SelBlocks Global also provides [EnhancedSyntax](EnhancedSyntax).

# Differences to SelBlocks #

## Doubling of back apostrophe \`
If parameters of your Selenese actions contain back apostrophe \`, you need to double it into \`\`. See [EnhancedSyntax](EnhancedSyntax).

## Lexical scope ##
When calling a _function_, it doesn't inherit variables from the higher scope in SelBlocks Global. See [SelBlocks issue #5](https://github.com/refactoror/SelBlocks/issues/5).

## Strict mode ##
SelBlocks Global is in [JavascriptEssential](JavascriptEssential) > [Strict Javascript](JavascriptEssential#strict-javascript), which prevents some bad practice code. That also applies to Javascript expressions passed to SelBlocks Global Selenese commands, or passed through [EnhancedSyntax](EnhancedSyntax) notation \`...\` and #\`...\`. This implies the following incompatibilities with SelBlocks.

### Accessing stored variables ###
When accessing stored variables, use _$xyz_ rather than just _xyz_. SelBlocks Global had to drop shorthand syntax of SelBlocks that gave some commands access to stored variables without using $ prefix. (That depended on Javascript keyword _with(obj){...}_, which is [prohibited in strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/Strict_mode#Simplifying_variable_uses).) The affected Selenese commands are _for_ and _call_.

### Loop _for_ ###
_for_ loop now must use _$xyz_ notation for all stored parameters (loop iterator or any other), whether on the left side or right side of the assignment operator =. So, instead of

```
for|i=1; i<=10; i++
```

use

```
for|$i=1; $i<=10; $i++
```

### Passing parameters to functions via _call_ ###
_call_ must use _$xyz_, but only in expressions on the right side of parameter assignments _parameterName=expression_. The formal parameter names on the left (ones being passed to the function) must not start with $. So, instead of

```
call|myFunction|myParam=storedVariableInCallingScope
```

use

```
call|myFunction|myParam=$storedVariableInCallingScope
```

## Try/catch suppresses error counts ##
_try...catch_ suppresses error counts and some error logs for exceptions, errors or failures of asserts/verifications. This benefits scripts that test Selenese commands themselves (e.g. ones provided by SeLite or any custom commands).

# Flow control with Selenese boolean accessors
Selenium, SeLite and custom add-ons define _isXyz()_ Selenese boolean accessors. You may combine them with _if_, _elseIf_ or _while_, by passing _selenium.isXyz()_ or _selenium.isXyz('locatorString')_. Indeed, you may combine the accessor calls in more complex boolean expressions. For example:

```
if | !selenium.isVisible( 'id=pmf-navbar-collapse' )
```

@TODO DOC Selenese > ???: CombiningExpressions: Use variable _selenium_ in Selenium Core scope for the same as what _this_ keyword is in context of Selenese actions _getEval_ (and related), if, while. However, in their context _this===selenium_, hence use _selenium_ instead of _this_, so that it's the same as in Selenium Core scope. (In other scopes, e.g. in a Selenium IDE extension or a [Javascript code module](JavascriptComplex#javascript-code-modules), load _SeLiteMisc_ code module and use _SeLiteMisc.selenium_ instead).
-> So, unless you need a result of a Javascript expression in multiple places in the same Selenese part or the same Selenese function, don't call _storeEval_ but use [EnhancedSyntax](EnhancedSyntax) to pass the Javascript expression directly within paired backticks \`...\` (or =\`...\`, if the result can be other than a string). This minimises a need to use _storeEval_ and auxiliary stored variables. That makes Selenese scripts shorter and clearer.