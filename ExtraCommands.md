---
title: Extra Commands
layout: default
---

[SeLite Commands](https://addons.mozilla.org/en-US/firefox/addon/selite-commands/), one of SeLite [AddOns](AddOns), provides several Selenese commands and related functionality.

# 'Robust' commands #
Commands with name in form <em>xxxRobust: typeRobust, clickRobust, selectRobust</em> action the same as original commands _xxx_, but if the target doesn't exist, then they skip and they don't fail.

# Random data #
Commands with name in form <em>xxxRandom: clickRandom, selectRandom, typeRandom, typeRandomEmail</em> generate controlled random data. The commands enter (or select or click) a random value(or an option or a radio button) for a given field (of a specified type). Optionally, they can also store the entered/selected/clicked text/choice in a given Selenese variable, so that the test can use it later (e.g. to store it in test DB).

Those commands perform two functions

  * main purpose: click/select a random element (matching the given selector), or type random text (more below)
  * optionally, capture the value of that clicked/selected element, or capture the typed text, into a stored variable. That facilitates later stages (e.g. when you submit the form and load a view of the record, then you want can validate that the clicked/selected/typed data got submitted).

For that the commands have two parameters:

  * _selectLocator_ (or _radiosXPath_ or _locator_), required: a selector to match the set of elements, from which it chooses a random one
  * _store_ or _paramsOrStore_ (optional):
    * _store_ is a name of stored variable, where the command saves the clicked/selected/typed value. It may include field or sub(sub-...)-field e.g. _variableName.fieldName, variableName.fieldName.subfieldName..._
    * _paramsOrStore_ can be either
      * a string: a name of stored variable, just like _store_ above
      * an object with one or multiple fields. Following examples use [SelBlocksGlobal](SelBlocksGlobal) and its [EnhancedSyntax](EnhancedSyntax) to pass objects through =\`...\` notation.

_typeRandomEmail_ co-operates with _typeRandom_. It types a random email address, based on a name already typed in another element.

For more details see [its Selenese tests](https://code.google.com/p/selite/source/browse/#git%2Fcommands%2Fselenese-tests).

# Timestamp-related commands #
There are two sets of functionality that support [TimeStamps](TimeStamps). The first set defines commands (primary names): <em>sleepUntilTimestampDistinctDownToMilliseconds, sleepUntilTimestampDistinctDownToSeconds, sleepUntilTimestampDistinctDownToMinutes</em>. Each ensures that a timestamp from that moment will be unique, when compared to any timestamp created just before any previous or future call to the same command (or to a command with finer precision).

The second set defines functions <em>isTimestampDownToMilliseconds, isTimestampDownToSeconds, isTimestampDownToMinutes, isTimestampDownToPrecision</em>. You can't access those directly as commands in Selenium IDE. Instead, use commands like _verifyTimestampDownToSeconds_ (see also [SeleniumIDE](SeleniumIDE) > [Auto-generated Selenese commands](SeleniumIDE#auto-generated-selenese-commands)). Those serve to validate a displayed timestamp (identified by locator in _target_ parameter) against a previously saved timestamp (passed in _value_ parameter).

The second set also auto-generates commands like _waitForTimestampDownToSeconds_. However, do not use those commands because they could be misplaced with _sleeUntilTimestampDistinctDownToSeconds_. To prevent confusion, this subset of auto-generated commands (_waitForTimestampDownToSeconds_ and similar) are handled specially: they fail. If you need to wait for a timestamp and to validate it, use a different _waitFor..._ command (targeting the related element), and then verify or assert the timestamp (with e.g. _assertTimestampDownToSeconds_).

## Basic usage ##
  * trigger a change
  * _storeTimestampDownToSeconds_ (or similar - for the chosen precision)
  * _sleepUntilTimestampDistinctDownToSeconds_ (or similar)
  * load a view
  * _assertTimestampDownToSeconds_ (or _verifyTimestampDownToSeconds_) against the stored timestamp

# Other commands #
  * _disableJavascript, enableJavascript_: disable/enable Javascript for the web application that is being tested
  * _indexBy_ - index objects
  * _selectTopFrameAnd_

# Reference #
For details see reference of those commands in Selenium IDE, [online](https://cdn.rawgit.com/selite/selite/master/commands/src/chrome/content/reference.xml) or locally at [_chrome://_ URL](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) _chrome://selite-extension-sequencer/content/selenese_reference.html?chrome://selite-commands/content/reference.xml_.