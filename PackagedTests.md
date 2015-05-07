---
title: Packaged tests
layout: default
---

# Purpose #
This describes how to use and create Selenese tests packaged with configuration (and with local forms/pages, if needed), and Javascript tests. Such tests validate functionality of some [AddOns](AddOns) and application-specific frameworks. They also serve as practical documentation, ready for inspecting at any detail in Selenium IDE.

# Selenese tests #

## Structure of tests ##

### Installing and getting around ###
Follow [InstallFromSource](InstallFromSource) for the easiest way to get Selenese tests. [AddOns](AddOns) that can be tested in Selenium IDE have subfolder _selenese-tests_, e.g. [commands/selenese-tests/](https://code.google.com/p/selite/source/browse/#git%2Fcommands%2Fselenese-tests). Each framework that comes with SeLite has test suites in its subfolder _test\_suites\_and\_cases_, e.g. [phpmyfaq/test\_suites\_and\_cases/](https://code.google.com/p/selite/source/browse/#git%2Fphpmyfaq%2Ftest_suites_and_cases).

Tests of add-ons and frameworks consist of test suites, test cases and related HTML forms/pages. To make navigation across files easy, here's a convention: filenames of test suites end with _'\_suite.html'_, and test cases are in files that have names ending with _'\_case.html'_. If there are several shared test cases, they can be in _shared\_test\_cases_ subfolder.

If a test needs configuration (through [Settings](Settings)), it has [SettingsManifests](SettingsManifests) > ['Values' manifests](SettingsManifests#-values-manifests). If the same add-on has multiple test suites that need different configuration, such test suites and their 'values' manifests are in subfolders.

Selenium IDE doesn't indicate the current test suite's folder in the GUI. Therefore make test suite file names clear. That helps especially when having similar test suites in different folders (because of different configurations). SeLite own test suites have file names in format <em>add-on-name_subfolder_suite.html</em> or <em>add-on-name_subfolder_subfolder_suite.html</em>.

### Navigating to local forms/pages ###
Tests can contain files (HTML, Javascript etc.) and can refer to them no matter where on your filesystem you have them. That works through _file://_-based URLs relative to location of the test suite. Such URLs should not be relative to the test case, because the same test case can be shared by multiple test suites.

Reasoning: A test case can open local pages relative to its location, or to location of the current test suite (the one loaded). Either choice has some positives, and can also be confusing. However, using URLs relative to the current test suite allows flexible re-use of test cases and their functions, with customised versions of local pages for each test suite. If all such test suites can use same version of a local page, such a file can be in a common parent folder. This results in similar test suites having the same folder structure and same names for local pages, which simplifies navigation.

Test suites that share test cases from parent folder(s) should be at the same directory depth, so that they can access any local .html files in higher folders through same relative URLs (e.g. _../page.html_). Tests open local forms/pages by e.g.

```
TODO FIX open | file://SeLiteSettings.getTestSuiteFolder()/form.html
```

### Negative tests ###
If you need to test that something fails, use <em>try..catch..endTry</em> as per [SelBlocksGlobal](SelBlocksGlobal). Use <em>catch..endTry</em> to set a stored variable of your choice. Check that variable after _endTry_ (e.g. with _getEval_) and throw an error on failure. Remember to clear that stored variable _before_ that block of steps, so that it can run multiple times.

There are also 'real' negative tests in _selite.sel-blocks-global/selenese-tests-negative/_. Those are supposed to fail, since they validate cases when <em>try..catch..endTry</em> doesn't catch an error.

## Invoking tests ##

### Running a test suite ###
Before you run a test packaged with local forms/pages, the current tab in Firefox must be blank (or showing something loaded through _file://..._ URL). If it shows anything from _http://_ or _https://_, the test will fail (it won't be able to access any local files).

Open a test suite using Selenium IDE menu File > Open... or Open Test Suite... Do not open actual test cases via menu File > Open..., otherwise they won't be able to access any local forms/pages (as per above) nor any shared Selenese functions. Then you can run the whole test suite, or selected test cases. Shared test cases that only define Selenese functions are not intended as runnable.

### Running multiple test suites ###
In order to run the whole set of SeLite test suites (except for _selite.sel-blocks-global/selenese-tests-negative/_), use [AddOns](AddOns) > Run All Favorites. Import _selite/run-all-favorites.json_. You need source of SeLite and [SelBlocksGlobal](SelBlocksGlobal) to be in their default folders (_selite_ and _selite.sel-blocks-global_) under your home folder. Those folder names are the default when you [check them out](https://code.google.com/p/selite/source/checkout) from GIT. If you download them instead, rename the folders to _selite_ and _selite.sel-blocks-global_.

# Javascript tests #
These validate functionality

  * at lower level than Selenese tests, or
  * which would be awkward to test in Selenese, or
  * which is not Selenese-specific, and while it can (and should) be tested from Selenese, it should be tested without Selenium IDE, too.
There's no extra installation - they are a part of their add-ons (in folders _javascript-tests_). You can invoke them directly (without Selenium IDE) through [_chrome://_ URLs](AboutDocumentation#firefox-chrome-urls-for-documentation-and-gui) listed at [AddOns](AddOns). However, you normally don't need to run them separately, since they get invoked from packaged Selenese tests.

# Shell tests #
These are invoked from shell (i.e. _cmd_ on Windows). They only exist for [ExtensionSequencer](ExtensionSequencer) > [Shell tests](ExtensionSequencer#shell-tests).