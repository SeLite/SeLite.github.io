---
title: Selenium flow
layout: default
---

~~~
Changing a test case:
dblclick -> showTestCaseFromSuite() -> setTestCase()

Playing tests:
'cmd_selenium_play'-> Editor.prototype.playCurrentTestCase(null, 0, 1) -> compileSelBlocks
-- once per each test case: showTestCaseFromSuite() -> setTestCase()
-- after the loop, restore by calling showTestCaseFromSuite() with testCaseOriginalIndex
~~~