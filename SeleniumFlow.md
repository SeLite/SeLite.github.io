---
title: Selenium flow
layout: default
---

~~~
Changing a case:
dblclick handler -> showTestCaseFromSuite() -> setTestCase()

Playing scripts:
'cmd_selenium_play'-> Editor.prototype.playCurrentTestCase(null, 0, 1) -> compileSelBlocks
-- once per each case: showTestCaseFromSuite() -> setTestCase()
-- after the loop, restore by calling showTestCaseFromSuite() with testCaseOriginalIndex
~~~