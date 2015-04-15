---
title: Selenium flow
layout: default
---

<pre>
Changing a test case:<br>
dblclick -> showTestCaseFromSuite() -> setTestCase()<br>
<br>
Playing tests:<br>
'cmd_selenium_play'-> Editor.prototype.playCurrentTestCase(null, 0, 1) -> compileSelBlocks<br>
-- once per each test case: showTestCaseFromSuite() -> setTestCase()<br>
-- after the loop, restore by calling showTestCaseFromSuite() with testCaseOriginalIndex<br>
</pre>