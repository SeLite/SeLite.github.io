---
layout: default
---
* TOC
{:toc}

# Locating a Javascript function in sources #
There are various ways of how to define/set/override a function in Javascript (usually through setting it as if it were a field on the class prototype object). So if you need to debug/modify/extend a function, it may not be easy to locate. You can use the following regular expressions to find definition(s) of a function (if implemented in one of the common ways). Replace `FUNCTION` with the name of the function.

The regex itself, e.g. for use in NetBeans:

```
function\s+FUNCTIONNAME[^a-zA-Z0-9_]|[^a-zA-Z0-9_]FUNCTIONNAME\s*[:=]\s*function|\[\s*['"]FUNCTIONNAME['"]\s*\]\s*=\s*function
```

The same regex escaped for use in bash:

```
egrep -r "function\s+FUNCTIONNAME[^a-zA-Z0-9_]|[^a-zA-Z0-9_]FUNCTIONNAME\s*[:=]\s*function|\[\s*['\"]FUNCTIONNAME['\"]\s*\]\s*=\s*function" *
```

This is especially useful with Selenium which uses same name functions in various classes/components. There may be various versions of the same class/component, depending on how the code is executed - via Selenium IDE or via webdriver (but only Selenium Core and IDE is relevant to SeLite).

In Selenium IDE sources search for class definitions with regex:

```
className *= *classCreate *\(
```

# Function intercepts
This is for extending or completely replacing behaviour of existing functions that come from Selenium or third party. It can be done for ordinary (non-member) functions (including class constructors) and also for methods (member functions of objects). Methods can be intercepted on either

  * the class prototype (which is more transparent, simpler and preferable, where possible), or
  * an instance, which is more complex (you may also need to intercept the class constructor, and make it set up intercept of the actual function that you intend to modify). It's needed when there's no robust access to the prototype.

## _Head/tail_ intercepts
Extending (rather than replacing) is useful for small changes, and it's more likely to stay compatible with future updates from Selenium or third party. The original function is stored. The new function adds steps before and/or after calling the original function (with the same or different parameters). This approach is called _head_ or _tail_ intercept. If it's supposed to return a value, it can return the result of the original function, or something different.

Head intercept with a modification of the parameter passed to the original function:

```javascript
"use strict";
// The original function (it would come from Selenium or third party)
function greeting( name ) {return "Hello " +name;}

// Anonymous closure to keep 'originalGreeting' variable local
( function() {
    var originalGreeting= greeting;

    greeting= function greeting( name ) {
        if( !name ) {
            name= "guest";
        }
        return originalGreeting.call( null/*'this' object*/, name );
    };
  }
) ();
```

Tail intercept:

```javascript
"use strict";
// The original function (it would come from Selenium or third party)
function greeting() {return "Hello";}

// Anonymous closure to keep 'originalGreeting' variable local
( function() {
    var originalGreeting= greeting;

    greeting= function greeting() {
        var originalResult= originalGreeting.call();
        return originalResult+ ", my friend.";
    };
} ) ();
```

If you use SeLite Bootstrap, see also [Bootstrap](Bootstrap) > [Intercepts](Bootstrap#intercepts).

# Using synchronous DB API #
Mozilla offers both synchronous and asynchronous API for accessing SQLite. I may have misunderstood their documentation and examples of using asynchronous calls, but it seems highly impractical to me: the only way to have a sequence of operations seemed to chain handler functions (one per each operation). Since the handlers are called from a separate thread (or after the main execution loop cycle?), there was no easy way to have the main thread wait for the result(s). And since cross-thread synchronisation is not defined in Javascript, it was all strange.

Also, synchronous API itself is a bit easier to use

  * no need for callback handler functions
  * when using `executeStep()` with `SELECT`, the rows are objects with fields named after DB columns
  * when using asynchronous API with `SELECT`
    * the rows don't contain object fields named after DB columns, so you must use methods to access the fields - see [mozIStorageStatement.executeAsync()](https://developer.mozilla.org/en/mozIStorageStatement#executeAsync()) and [mozIStorageRow](https://developer.mozilla.org/en/mozIStorageRow)
    * `handleResult()` callback may be called several times per same statement!