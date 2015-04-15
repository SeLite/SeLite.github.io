---
title: Javascript other
layout: default
---

# Using synchronous DB API #
Mozilla offers both synchronous and asynchronous API for accessing SQLite. I may have misunderstood their documentation and examples of using asynchronous calls, but it seems highly impractical to me: the only way to have a sequence of operations seemed to chain handler functions (one per each operation). Since the handlers are called from a separate thread (or after the main execution loop cycle?), there was no easy way to have the main thread wait for the result(s). And since cross-thread synchronisation is not defined in Javascript, it was all strange.

Also, synchronous API itself is a bit easier to use
  * no need for callback handler functions
  * when using executeStep() with SELECT, the rows are objects with fields named after DB columns
  * when using asynchronous API with SELECT
    * the rows don't contain object fields named after DB columns, so you must use methods to access the fields - see https://developer.mozilla.org/en/mozIStorageStatement#executeAsync() and https://developer.mozilla.org/en/mozIStorageRow
    * handleResult() callback may be called several times per same statement!

# Field name 'toString' #
Javascript objects representing test DB records have method 'toString' for convenience. It means that there should be no DB column with name 'toString'.