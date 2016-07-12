---
layout: default
---
{% include links %}
* TOC
{:toc}

# Packaged scripts
Packaged scripts are Selenese [scripts][script] together with any [Settings](Settings) and optional local `.html` forms/pages. They usually also use a framework.

In SeLite these scripts validate functionality of [Components](Components) and application-specific frameworks. They also serve as practical documentation, ready for inspecting at any detail in Selenium IDE.

## Structure

## Installing and getting around ###
Follow [InstallFromSource](InstallFromSource) for the easiest way to get SeLite packaged scripts. [Components](Components) that can be tested in Selenium IDE have subfolder `selenese-scripts`, e.g. [commands/selenese-scripts/](https://github.com/SeLite/SeLite/tree/master/commands/selenese-scripts). Each framework that comes with SeLite has [suites][suite] in its subfolder `test_suites_and_cases`, e.g. [phpmyfaq/test\_suites\_and\_cases/](https://github.com/SeLite/SeLite/tree/master/phpmyfaq/test_suites_and_cases).

To make navigation across files easy, here's a convention: filenames of suites end with `_suite.html`, and cases are in files that have names ending with `_case.html`. If there are several shared cases, they can be in `shared_cases/` subfolder.

Scripts have {{navValuesManifests}} for any [Settings](Settings). If the same component (add-on) has multiple [suites][suite] that need different configuration, such [suites][suite] and their {{valuesManifest}}s are in subfolders.

Selenium IDE doesn't indicate the current [suite]'s folder in the GUI. Therefore make suite file names clear. That helps especially when having similar suites in different folders (because of different configurations). SeLite own suites have file names in format `component-name_subfolder_suite.html` or `component-name_subfolder_subfolder_suite.html`.

## Navigating to local forms/pages
[Cases][case] of packaged scripts must refer to their local `.html` forms/pages by _file://_-based URLs relative to location of the [suite]. Do not use URLs should not be relative to the [case], because the same case can be shared by multiple suites.

Reasoning: A case can open local pages relative to its location, or to location of the current suite (the one loaded). Either choice has some positives, and can also be confusing. However, using URLs relative to the current suite allows flexible re-use of automation cases and their Selenese functions, with customised versions of local pages for each [suite]. If all such suites can use same version of a local page, such a file can be in a common parent folder. This results in similar suites having the same folder structure and same names for local pages, which simplifies navigation.

[Suites][suite] that share [cases][case] from parent folder(s) should be at the same directory depth, so that they can access any local `.html` files in higher folders through same relative URLs (e.g. `../page.html`). Scripts open local forms/pages by e.g. `open | file://<> SeLiteSettings.getTestSuiteFolder() <>/form.html`.

We indent commands in tests. That requires [SeLite Clipboard And Indent](https://addons.mozilla.org/en-US/firefox/addon/selite-clipboard-and-indent/). Otherwise run `sed -r -e 's/<td>(&nbsp;)+/<td>/' test-case-location >temp_out; cp temp_out >test-case-location`.

## Negative tests that succeed
If you need verify that a Selenese command fails under certain circumstances, put it within ``try..catch` of a `try..catch..endTry` block as per [SelBlocksGlobal](SelBlocksGlobal). Use its `catch..endTry` part to set a stored variable of your choice. Check that variable after `endTry` (e.g. with `getEval`) and if not set, throw an error. Remember to clear that stored variable **before** that block of steps, so that it can run multiple times.

## Negative tests that fail
There are real negative tests in e.g. `selite.sel-blocks-global/selenese-scripts-negative/`. Those tests themselves are supposed to fail (e.g. they validate situations when `try..catch..endTry` is supposed not to catch an error). If these tests don't fail, then there's a problem.

# Invoking scripts

## Running a suite
Before you run a [script] packaged with local forms/pages, the current tab in Firefox must be blank (or showing something loaded through _file://..._ URL). If it shows anything from _http://_ or _https://_, the script will fail (it won't be able to access any local files).

Open a suite using Selenium IDE menu File > Open... or Open Test Suite... Do not open actual [cases][case] via menu File > Open..., otherwise they won't be able to access any local forms/pages (as per above) nor any shared Selenese functions. Then you can run the whole suite, or selected cases. Shared [cases][case] that only define Selenese functions are not intended as runnable.

## Running multiple suites
In order to run the whole set of SeLite [suites][suite] (except for `selite.sel-blocks-global/selenese-scripts-negative/`), use [Components](Components) > Run All Favorites. Import `SeLite/run-all-favorites.json`. You need source of SeLite and [SelBlocksGlobal](SelBlocksGlobal) to be in their default folders (`SeLite` and `SelBlocksGlobal`) under your home folder. Those folder names are the default when you [check them out](https://github.com/SeLite/Selite) from GIT. If you download them instead, rename the folders to `SeLite` and `SelBlocksGlobal`.<!-- TODO Test. replace here and elsewhere -->

# Javascript tests #
These validate functionality

  * at lower level than Selenese tests, or
  * which would be awkward to test in Selenese, or
  * which is not Selenese-specific, and while it can (and should) be tested from Selenese, it should be tested without Selenium IDE, too.

There's no extra installation - they come as a part of their components (in folders `javascript-tests`). You can invoke them offline and directly from Firefox (without starting Selenium IDE) through {{chromeUrl}}s listed at [Components](Components). However, you normally don't need to run them separately, since they get invoked from packaged Selenese scripts.

# Shell tests #
These are invoked from shell (i.e. `cmd` on Windows). They only exist for [ExtensionSequencer](ExtensionSequencer) > [Shell tests](ExtensionSequencer#shell-tests).