---
title: SelBlocksGlobal
layout: default
---

# Overview #
SelBlocks Global is one of SeLite AddOns. It's an enhancement of
[SelBlocks](https://addons.mozilla.org/en-US/firefox/addon/selenium-ide-sel-blocks/versions/). SelBlocks allows reusing blocks of steps grouped into functions (formerly called 'scripts'). However, it only lets you call the functions from within the same test case.

If we could call functions across test cases, we could organise them better than in one long file. A test case can be a part of multiple test suites. So we could share functions between test suites, too. That means less copy and paste code. If you structure your functions well, you'll have
  * less maintenance
  * more compact tests
  * higher productivity.

SelBlocks Global allows you to do that. It can call functions from other test cases (within the same test suite). Since a test suite can contain any test cases (from anywhere on the filesystem), you can share test cases with functions between any test suites.

It replaces SelBlocks. You can't use SelBlocks Global together with SelBlocks (or FlowControl, GoTo or Sideflow).

# Documentation #
Original documentation of SelBlocks applies to SelBlocks Global (except for incompatibility listed below). See summary of <a href='https://addons.mozilla.org/en-US/firefox/addon/selenium-ide-sel-blocks/'>SelBlocks</a> extension itself and its full <a href='http://refactoror.wikia.com/wiki/Selblocks_Reference'>documentation</a>

See [SelBlocksGlobal Selenese reference](http://sel-blocks-global.selite.googlecode.com/git/src/chrome/content/reference.xml). In addition to those commands, SelBlocks Global also provides [EnhancedSyntax](EnhancedSyntax).

## Clarification of term 'function' ##
Word 'function' can refer to a Javascript function, or to a function defined by SelBlocks Global/SelBlocks construct _function...endFunction_. If it's unclear, let's call the later 'test case function'.

# Differences to SelBlocks #
## Doubling of back apostrophe ````` ##
If parameters of your Selenese actions contain back apostrophe `````, you need to double it into ````````. See EnhancedSyntax.

## Lexical scope ##
When calling a _function_, it doesn't inherit variables from the higher scope in SelBlocks Global. See [SelBlocks issue #5](https://github.com/refactoror/SelBlocks/issues/5).

## Strict mode ##
SelBlocks Global is in [JavascriptEssential](JavascriptEssential) > [Strict Javascript](JavascriptEssential#strict-javascript), which prevents some bad practice code. That also applies to Javascript expressions passed to SelBlocks Global Selenese commands, or passed through [EnhancedSyntax](EnhancedSyntax) notation ```...``` and ``#`...```. This implies the following incompatibilities with SelBlocks.

### Accessing stored variables ###
When accessing stored variables, use _$xyz_ rather than just _xyz_. SelBlocks Global had to drop shorthand syntax of SelBlocks that gave some commands access to stored variables without using $ prefix. (That depended on Javascript keyword _with(obj){...}_, which is [prohibited in strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/Strict_mode#Simplifying_variable_uses).) The affected Selenese commands are _for_ and _call_.

### Loop _for_ ###
_for_ loop now must use _$xyz_ notation for all stored parameters (loop iterator or any other), whether on the left side or right side of the assignment operator =. So, instead of
```
for|i=1; i<=10; i++```
use
```
for|$i=1; $i<=10; $i++```

### Passing parameters to functions via _call_ ###
_call_ must use _$xyz_, but only in expressions on the right side of parameter assignments `parameterName=expression`. The formal parameter names on the left (ones being passed to the function) must not start with $. So, instead of
```

call|myFunction|myParam=storedVariableInCallingScope
```
use
```

call|myFunction|myParam=$storedVariableInCallingScope
```
## Try/catch suppresses error counts ##
_try...catch_ suppresses error counts and some error logs for exceptions, errors or failures of asserts/verifications. This benefits scripts that test Selenese commands themselves (e.g. ones provided by SeLite or any custom commands).
