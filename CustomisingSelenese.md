---
title: Customising Selenese
layout: default
---

See also [SeleniumIDE](SeleniumIDE) > [Auto-generated Selenese commands](SeleniumIDE#auto-generated-selenese-commands).

# Locating Javascript implementation of Selenese 'doer' commands #
Selenese primary 'doer' commands (i.e. ones with primary forms that don't start with _**get**_ not _**is**_ and that are not like <em><strong>is</strong>Xyz<strong>Present</strong></em> - as per table in [SeleniumIDE](SeleniumIDE) > [Auto-generated Selenese commands](SeleniumIDE#auto-generated-selenese-commands)) are defined in Javascript functions whose names start with _**do**_. E.g. command _xyz_ is implemented in function <strong>do</strong>Xyz_. Therefore do not search for their implementation functions by their Selenese names case sensitively._

# Locating a Javascript function in sources #
There are various ways of how to define/set/override a function in Javascript (usually through setting it as if it were a field on the class prototype object). So if you need to debug/modify/extend a function, it may not be easy to locate. You can use the following regular expressions to find definition(s) of a function (if implemented in one of the common ways). Replace _FUNCTION_ with the name of the function.

The regex itself, e.g. for use in NetBeans:

```
function\s+FUNCTIONNAME[^a-zA-Z0-9_]|[^a-zA-Z0-9_]FUNCTIONNAME\s*[:=]\s*function|\[\s*['"]FUNCTIONNAME['"]\s*\]\s*=\s*function
```

The same regex escaped for use in bash:

```
egrep -r "function\s+FUNCTIONNAME[^a-zA-Z0-9_]|[^a-zA-Z0-9_]FUNCTIONNAME\s*[:=]\s*function|\[\s*['\"]FUNCTIONNAME['\"]\s*\]\s*=\s*function" *
```

This is especially useful with Selenium which uses same name functions in various classes/components. There may be various versions of the same class/component, depending on how the code is executed - via Selenium IDE or via webdriver (but only Selenium Core and IDE is relevant to SeLite).

# Defining functions in Selenium Core #
This is for files normally loaded into Selenium Core/Selenese scope (via [BootstrapLoader](BootstrapLoader) or [ExtensionSequencer](ExtensionSequencer), which use [JavascriptComplex](JavascriptComplex) > [mozIJSSubScriptLoader](JavascriptComplex#mozIJSSubScriptLoader)). (Those are not [Javascript code modules](JavascriptComplex#javascript-code-modules).)

You can define functions for Selenese scope in either way mentioned at [JavascriptEssential](JavascriptEssential) > [Defining Javascript functions](JavascriptEssential#defining-javascript-functions). However, if you use [the classic way](JavascriptEssential#the-classic-way), do that only in _strict_ mode. Otherwise the function will be in the Selenium global scope (i.e. outside of Selenium Core/Selenese scope) - then you need to see [ExtensionSequencer](ExtensionSequencer) > [Core extensions loaded twice](ExtensionSequencer#core-extensions-loaded-twice).

# Special rules for custom Selenese commands #

## Names of 'doer' commands ##
Don't have primary Selenese 'doer' actions (e.g. action _Abc_, implemented by Javascript function <em><strong>do</strong>Abc</em>) with names in form <em><strong>get</strong>Xyz, <strong>is</strong>Xyz</em> or <em><strong>is</strong>Xyz<strong>Present</strong></em>  (i.e. implemented by functions <em><strong>doGet</strong>Xyz</em>, <em><strong>doIs</strong>Xyz</em> or <em><strong>doIs</strong>Xyz<strong>Present</strong></em>), unless you have a very good reason. Such names would imply that the command is a 'getter' or 'checker' rather than a 'doer'. They would suggest that there are other auto-generated Selenese commands for them (_**assert**Xyz_ etc., as per [SeleniumIDE](SeleniumIDE) > [Auto-generated Selenese commands](SeleniumIDE#auto-generated-selenese-commands)), but those wouldn't exist.

## Getter commands ##
Don't define the second parameter (usually called 'value') for Selenese 'getter' commands. See https://code.google.com/p/selenium/issues/detail?id=3202.