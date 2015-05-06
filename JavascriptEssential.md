---
title: Javascript essentials
layout: default
---

# General notes #
  * Design and code for clarity, simplicity and robustness (but within reason). Don't optimise minor parts or if optimisation adds much complexity.
  * Use meaningful symbols and objects.
  * Perform checks early. When they fail, generate errors. Let errors show up, don't silence them.
  * Write documentation before implementation. You will benefit from it yourself:
    * This extra time enables you to reflect on what you want to achieve at higher level.
    * Reflecting on what you intend to implement can save you unnecessary work (immediately or later).
    * Complexity makes coding often slow and frustrating. By documenting first, once implemented you will be free to relax without a feeling of duty for 'paperwork'.
  * Create tests to be run from Selenium IDE - see [PackagedTests](PackagedTests).
  * Don't use fixed line length - see [DocumentationStandard](DocumentationStandard). However, split complex expressions on multiple lines.

# Differences from Javascript for web pages #
If you've used Javascript only for web pages, you’ll find out some new terms and patterns here. This applies to development of Selenium IDE Core extensions (including SeLite frameworks) or Firefox extensions in general. They run in privileged mode. That provides extra features and it also sets some restrictions.

## Privileged Javascript files ##
Privileged Javascript controls (or extends or overrides) Firefox functionality. It can only come from [_chrome://_](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) or _file://_ URLs. That is why [BootstrapLoader](BootstrapLoader) and [SettingsInterface](SettingsInterface) (and [SettingsAPI](SettingsAPI)) can't load files over http (neither https). _chrome://_ URLs are governed by extension's _chrome.manifest_ (which maps a custom _chrome://xyz/_ URL prefix to a location within the extension).

## Scope ##
Javascript for web applications has only two levels of scope: global and local (within functions). On the other hand, Firefox avoids conflicts between extensions by running them with separate global scopes. However, they can share Javascript files, and when they load them, they can specify global scope used in those files. (The code from such files is shared, even though global scope of its usages is different. See [JavascriptComplex](JavascriptComplex) > [Loading Javascript files](JavascriptComplex#loading-javascript-files).)

## Prevent name conflicts ##
If you add custom functionality at Selenium Core scope, do it only for new Selenese commands or for use in parameters of Selenese commands. The later should be grouped in an object with a name unlikely to cause conflicts. Such an object serves as a namespace. You can have multi-level namespace objects, or group classes and functions in a Java package-like notation.

When adding functionality that doesn't need to be directly available to Selenese expressions, use [JavascriptComplex](JavascriptComplex) > [Javascript code modules](JavascriptComplex#javascript-code-modules).

If adding Selenese commands specific to a web application, give their names a prefix unlikely to cause conflicts.

## Strict Javascript ##
In Javascript files, have the following as the very first statement (at the top level):

```javascript
"use strict";
```

That adds extra checks that help to prevent errors and some bad practice code. Strict mode only applies to the code within that file; it doesn't apply to any functions called from there. See [MDN > Strict mode ](https://developer.mozilla.org/en/JavaScript/Strict_mode).

# Defining Javascript functions
See also [> MDN > Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions).

## The classic way ##
This is a definition by _function_ statement. When using [strict Javascript](#strict-javascript), have such definitions at file level only and not within other functions or conditional/loop blocks (see [MDN > Strict mode > Paving the way for future ECMAScript versions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode#Paving_the_way_for_future_ECMAScript_versions)).

```javascript
"use strict";
function myFunction( param, anotherParam... ) {
...
}
```

## By function expression ##
This applies [function expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function). It generates ['closures'](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#Closures), functions that can access non-global variables from outside their scope (i.e. from the scope that contains that _function_ expression). See examples below, at [Isolate the local scope](#isolate-the-local-scope) and [Function intercepts](#function-intercepts).

### Avoid nameless functions ###
This only applies to definitions by [function expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function). That's useful for adding/defining functions

  * on existing objects,
  * on existing prototypes or
  * [in the passed global scope](#passing-the-global-scope).
  * in [strict Javascript](#strict-javascript), where this the only way of defining functions from within other function or blocks.

The following three definitions use function expression. However, they are all nameless.

```javascript
"use strict";
var myFunction= function( param... ) { ... };

var myNameSpace= {};
myNameSpace.anotherFunction= function( param... ) { ... };

var object= {
memberFunction: function( param... ) { ... }
};
```

Such functions don't have a name. That doesn't affect the functionality, but:

  * It makes debugging (as per [DevelopmentTools](DevelopmentTools) > [Browser Toolbox](DevelopmentTools#browser_toolbox)) more difficult. In debugger's call stack, it shows these names. If you don't give a function a name, then you or others need to locate it by the source line.
  * For nameless constructors (i.e. functions that define classes):
    * If you inspect their instances, you need to add their _.constructor.toSource()_ to the Watch pane (as per [DevelopmentTools](DevelopmentTools) > [Source of functions](DevelopmentTools#source-of-functions)).
    * Their instances don't work with _SeLiteMisc.isInstance()_.

Prevent that by creating a named function (with the desired name) via [Named function expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function#Named_function_expression) and assign it to the target symbol. E.g.

```javascript
"use strict";
var myFunction= function myFunction( param... ) { ... };

var myNameSpace= {};
myNameSpace.anotherFunction= function anotherFunction( param... ) { ... };

var object= {
memberFunction: function memberFunction( param... ) { ... }
};
```

However, you don't need to name functions that exist only to [isolate the local scope](#isolate-the-local-scope). Those are an exception.

## Be careful with assigned/passed functions created the classic way ##
You could split the above suggested approach into a function definition ([the classic way](#the-classic-way)) and an assignment. E.g.

```javascript
"use strict";
function f() {return 1;}
var fn= f;
```

However, you shouldn't need that - if you assign it, use that variable to invoke (or access) the function.

Even worse, if you later redefine a function within the same scope, the new definition will apply to wherever you've assigned the old definition earlier! See:

```javascript
"use strict";
function f() {return 1;}
var first=f;

function f() { return 2;}
var second=f;

first(); // This returns 2 rather than 1!
```

That's when you have two or more definitions of the function with the same name (within the same scope), even though you save it to a different variable or to a different object/prototype. This can happen if you extend the existing code and you've forgot what methods there were already, or if someone else extends your code. It creates highly confusing conflicts.

# Function intercepts
This is for extending or completely replacing behaviour of existing functions that come from Selenium or third party. It can be done for ordinary (non-member) functions (including class constructors) and also for methods (member functions of objects). Methods can be intercepted on either

  * the class prototype (which is more transparent, simpler and preferable, where possible), or
  * an instance, which is more complex (you may also need to intercept the class constructor, and make it set up intercept of the actual function that you intend to modify). It's needed when there's no robust access to the prototype.

## Head/tail intercepts
Extending (rather than replacing) is useful for small changes, and it's more likely to stay compatible with future updates from Selenium or third party. The original function is stored. The new function adds steps before and/or after calling the original function (with the same or different parameters). Hence this approach is called 'head' or 'tail' intercept. If it's supposed to return a value, it can return the result of the original function, or something different.

Head intercept with a modification of the parameter passed to the original function:

```javascript
"use strict";
// The original function (it would come from Selenium or third party)
function greeting( name ) {return "Hello " +name;}

// Anonymous closure to keep 'originalGreeting' local
( function() {
var originalGreeting= greeting;

greeting= function greeting( name ) {
if( !name ) {
name= "guest";
}
return originalGreeting.call( null/*'this' object*/, name );
};
} ) ();
```

Tail intercept:

```javascript
"use strict";
// The original function (it would come from Selenium or third party)
function greeting() {return "Hello";}

// Anonymous closure to keep 'originalGreeting' local
( function() {
var originalGreeting= greeting;

greeting= function greeting() {
var originalResult= originalGreeting.call();
return originalResult+ ", my friend.";
};
} ) ();
```

If you use SeLite Bootstrap, see also [BootstrapLoader](BootstrapLoader) > [Intercepts](BootstrapLoader#intercepts).

# Isolate the local scope
Often, functions need variables/symbols that are global-like (or static-like), but you don't want such symbols in the global scope (which is the Selenese scope scope for Selenium Core extensions). This applies mostly to Selenium Core extensions and SeLite frameworks. (Javascript code modules have separate scopes, so they don't need this.)

You could have all your code defined within one long function, and then call it at the end of the file. However, if you name such a function, that name itself becomes part of the global scope.

The solution is to have such a (long) function, but make it anonymous. It then looks like

```javascript
"use strict";

var usefulExportedFunction;

( function() {
var localVariable;

usefulExportedFunction= function usefulExportedFunction( anyParameters... ) {
use localVariable here...
};
}
)();
```

## Passing the global scope
If you want a function (anonymous or not) to define global symbols, you can pass the global scope (i.e. ['global object'](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects)) to it. It's accessible via _this_ keyword at global scope (similar to _$GLOBALS_ in PHP).

So then the above example would be

```javascript
"use strict";

( function(global) {
var localVariable;

global.usefulExportedFunction= function usefulExportedFunction( anyParameters... ) {
use localVariable here...
};
}
)( this );
```

## Passing _this_
When you define a function [the classic way](#the-classic-way) (whether named or anonymous), _this_ keyword refers to the instance that the function will be invoked on (if any). But often you want that code to refer to _this_ as it were during execution of code that defined that function (i.e. _this_ from the scope that creates the new Function object by that _function_ statement).

You need to save that outer _this_ into a variable (usually called _self_) local in the scope outside of the new function (one being created by _function_ statement). Then use that variable (e.g. _self_) instead of _this_ where needed. See also [MDN > Operator this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) and [MDN > Functions > Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#Closures).

```javascript
"use strict";

function applyToArray( array, method ) {
for( var i=0; i<array.length; i++ ) {
method.call( null, array[i] );
}
}

function TestArrayWorker( array, multiplyBy ) {
this.array= array;
this.multiplyBy= multiplyBy;
}

TestArrayWorker.prototype.run= function() {
var self= this;
var result= 0;

applyToArray(
this.array,
function(item) {
result+= item*self.multiplyBy;
}
);
return result;
};

new TestArrayWorker( [1, 2], 5 ).run(); // This returns 15
```

# Documentation #
Document the functions, parameters, variables etc. in [JSDoc](http://usejsdoc.org) format. Apply JSDoc 3 when useful (even though NetBeans 7.3 only supports JSDoc 2). Some JSDoc tags are not obvious - e.g. use [@extends](http://usejsdoc.org/tags-augments.html) for documenting class inheritance. For documenting Javascript modules see [JavascriptComplex](JavascriptComplex).

Document why you do things, not what you do.

When commenting the code, do not use sequences of = (e.g. ======= or longer) as visual separators. ======= serves to separate conflict parts when a GIT merge (or SVN merge/update) causes conflicts.

If you intercept or extend Selenium, or if you figure out any non-trivial, non-obvious or unclear relationships between objects and/or functions, describe them as per [DocumentationStandard](DocumentationStandard) > [Textual object diagrams](DocumentationStandard#textual-object-diagrams).

# Other #

## No extra third party libraries
There's no need and no benefit from Google Closure with Selenium. Do not use JSLint, since it refuses iteration variables defined in _for_ loop to be used after the loop.

## Iterating over arrays ##
Firefox allows easy iteration over arrays, if you don't need to keep track of the index:

```javascript
var a=[1, 2];
for( var value of a) {
  // value is a value from a[]
}
```

However, NetBeans 8.0.1 doesn't support it (it breaks Navigator and block expand/collapse). See [NetBeans issue #237640](https://netbeans.org/bugzilla/show_bug.cgi?id=237640) and vote for it (and for the rest of ThirdPartyIssues). Until NetBeans gets fixed, SeLite uses the classic way

```javascript
var a=[1, 2];
for( var i=0; i<a.length; i++ ) {
var value= a[i];
}
```

## See also ##
  * [JavascriptComplex](JavascriptComplex)
  * [MDN: Coding Style](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Coding_Style)
  * [MDN: Javascript reference](https://developer.mozilla.org/en/JavaScript/Reference)