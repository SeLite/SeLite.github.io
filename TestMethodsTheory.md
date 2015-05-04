---
title: Test methods theory
layout: default
---

# Summary #
This describes and compares ways of test automation - from the data point of view. See also  pictures at [TestMethods](TestMethods).

# SeLite testing of DB-driven web apps #
It is testing of DB-driven applications, where the tests keep and update a copy of the application's DB. The tests themselves (the steps, conditional logic etc.) are not necessarily in a DB. The test-specific input data doesn't have to be in a DB (but it may be).

## Terms ##
These terms are not SeLite-specific. The goal here is to clarify possible/desirable features and connections between tests and tested systems. This mentions benefits and shortcomings of various approaches and of SeLite.

  * **application** - what is being tested, with
    * source, libraries, webserver, single sign on etc.
    * application data ('test app DB' or just 'app DB')
  * **test** - depending on the context, either
    * the system that tests (invokes) the application, i.e. Firefox + Selenium IDE + SeLite + optional custom Core extensions + test scripts
    * all your test scripts (cases and suites) that are run by the test system. They are grouped in test cases, where
      * a test case consists of steps
      * a test suite contains one or more test cases
      * one test case can belong to multiple test suites (and it can be re-used by cross-test case 'function' calls via [SelBlocksGlobal](https://addons.mozilla.org/en-US/firefox/addon/SeLite-SelBlocks-Global/versions/))
  * **data**
    * **'app data', 'app DB'** or just **'data'** - the application data (or its subset) relevant to testing
      * usually it means SQLite export of the application's DB
      * if the application uses SQLite, then it can be the app DB (SQLite file) itself (SQLite file on a network drive or local)
    * **'test data'** or **'test DB'**
      * what the test believes that the app data is, either
        * same source as app data - accessed by test read-only (not a good approach, see more below), or
        * a replica of app data, which
          * is separate - not accessed by the application
          * shadows app DB
          * gets updated by the test to reflect the changes expected to happen in the app data (see more below)
          * is used by test to navigate and verify the application
          * helps to detect and identify errors in the application or the test
          * is a core feature of SeLite
    * **'test input data'** or just **'input data'** is
      * data, that is used by the test, especially
        * for inputs/choices that the test enters/selects in the application, or
        * that determines webpage navigation during the test
      * can be
        * from XML using SelBlocks functionality (a part of [SelBlocksGlobal](SelBlocksGlobal))
        * from SQLite with the help of SeLite object-oriented layer
        * random, within a range or from a list of choices, using SeLite [ExtraCommands](ExtraCommands)
  * **test session**
    * custom runtime variables in the test system, which serve for
      * tracking web app user session and page navigation
      * temporary storage of loaded/modified test data and generated test input data
      * custom test  control
      * loading/updating records in test DB
  * **knock-on/silent defects** are 'hidden' or mysterious errors, whose buggy effect
    * doesn't show up immediately at all, or
    * is not initially tested or obvious (because of the scope, complexity etc.)
    * but the defect causes incorrect data which affects subsequent operation

## The goal ##
SeLite is for thorough, yet practical testing of DB-driven apps. Your tests
  * can validate screens based on (a replica of) all data, rather than based on just recent actions that only modify a part of data
  * don't need to specifically track the history of actions from previous test sessions
  * have to update their own DB to reflect those actions
    * where needed (they can skip changes done by the application to fields not used by the test - e.g. history logs)
  * need to be repeatable and robust
    * i.e. not specific to a test session or to a browser's session
    * the data is flexible and not trivial, e.g.
      * based on the test DB, rather than just hard-coded or sequential
      * random, semi random or partially random
    * actions/groups of actions may be done in random order
For example, you don't need SeLite  if your test just
  * creates a new record
  * navigates to/locates the record somehow (possibly by creation time)
  * validates that the record reflects what the test just has done to it
  * ignores the rest (other records, totals or averages etc.)
because that
  * doesn't need data from other records, and
  * it won't need the data of this new record, when it's re-run next time.
But you'll benefit from SeLite if you continually test the application, when you need the test to incorporate effect of actions done by all previous testing.

# Types of automated testing of DB-driven web applications #
This is not Selenium IDE or SeLite-specific, but it only covers what can be done with Selenium. There are a few options, the easiest first:

## 1. Test uses no DB ##
It's hard to make this thorough testing. The app DB
  * has initial state 'frozen' to make the test work
  * needs to be reset to that initial state (every time the test is run)
The test
    * has no own DB
    * has no access to app DB (except for access to reset it - if needed and automated)
    * depends on the initial app DB to be in the expected state
    * enters/modifies the app data
      * by navigating the app via its UI
      * based on fixed/random/partially random data set
      * stores the data in test session (if needed)
      * validates the data presented by the app UI (against that session data or against the fixed data)
    * doesn't know about any previously entered/modified data present in app DB, if (re)started

This is probably how many initial/proof-of-concept tests work. Not feasible for long term.

## 2. Web app and its test use the same DB ##
Like #1, but the test reads from the app DB.
The app DB
  * has no 'frozen' initial state
  * may need resetting/partial resetting from time to time
The test
  * has no own DB
  * has read-only access (back door) to app DB (and access to reset it - if needed and automated)
  * generates a fixed/random/controlled random data set, which it enters/modifies via the app UI
  * it validates the data presented by the app UI against
    * the app data (reloaded from the app DB)
      * possibly not detecting hidden/silent application errors that
        * cause app to save incorrect/incomplete/inconsistent information
        * pass unnoticed (unless this errornous data gets detected later but still in the same test session)
    * the test session
      * possibly detecting hidden errors, but it
        * is difficult to implement, if an error shows up only several steps/stages after its initial cause
        * can't identify errors caused by previous test runs (not tracked in the current test session)
  * if (re)started, it gets the initial state based on the current app DB only, with no other track of the changes caused by previous test runs

Single source of truth - i.e. same DB used by the app and by the test - causes hidden problems (false positives). The test may succeed, but the data flow has bugs.

The hidden data error may be detected later, but only if
  * there is a way to determine (re-calculate...) the correct value from the rest of the data and
  * the application presents the rest of the data needed for this (it may include pagination over the records...) and
  * the tester thinks of comparing the actual value and the expected value.
But, it's difficult to creates tests to cover this. So it would involve human validation.

Even worse, the data field (or fields) affected by the hidden bug may be not be verifiable by the rest of the data at all. That's usually when the DB schema doesn't cover the history of actions/transactions, or it removes old actions/transactions and it doesn't cover the intermediary steps. For example, a banking application could keep only the current amount and transaction amounts (credits/debits) for a fixed period, but it doesn't include amounts current at those transaction dates. If there's an error in applying a certain type of transaction and it results in an incorrect update of current amount, there's no way to detect this error - unless you have a separate copy of the data.

See [simple online demo](http://htmlpreview.github.io/?https://github.com/selite/selite/blob/master/demo/simple/web/index.html) as an example.

## 3. Web app and its test have separate DBs ##
This explains [Overview](./) > [Advantages of test data separation](./#advantages-of-test-data-separation). It's like #2, but the test
  * has its own DB
    * which is initially a copy of the app DB, with
      * schema
        * either exactly same as in the test DB, or
        * modified but logically equivalent to that of test DB, or
        * simplified or a subset of test DB schema - as relevant to testing
      * data
        * containing all data from the app DB, or
        * being a narrowed subset of the app DB data
          * then the test needs to be aware that the app may show more entries than what is in the test DB, i.e. the test needs to filter/scroll/navigate across the page(s) of records shown by the app, to locate the records that it wants to test
        * should be approximately equal to the app data (for approximate fields see [HandlingData](HandlingData) and [TimeStamps](TimeStamps))
  * keeps its DB in sync with app DB (or its part), updating test DB to reflect changes in app DB
    * but it doesn't blindly copy/replicate the changes from app DB to test DB (via a back door), since that would effectively be approach #2
    * the test updates its DB on its own, but in a way that the tester believes the app updates its DB
    * ideally the test doesn't use exactly same SQL queries as the app, but ones that are logically equivalent
      * this helps to detect SQL-level logical bugs (other than syntax errors)
      * SeLite helps with this, by providing an object oriented layer (which is unlikely to be used by the web app, therefore the app uses a different method to generate SQL, so there's a higher chance to detect an error in the app)
  * validates the data presented by the app UI against the data in the test DB, which enables the test to detect hidden/silent application errors
    * whether during the test run when the error happens, or during a later test
  * the test DB may get out of sync if
    * the test is incomplete/incorrect (being developed), or
    * the app and/or test dies/times out
    * the app DB or test DB is changed in a way other than through the test (likely during development of the test or the app)
  * and then
    * the test DB needs to be reloaded from new a copy of the app DB (if healthy), or
    * both test and app DBs needs to be reset
    * potential challenge: automate this DB reload/reset

If the same error exists in both the app and the test, then it doesn't get detected automatically. That may be at various levels
  * DB schema
    * because the app and the test DB schema are same/equivalent
    * there's no big need to detect these errors by automated testing, since the schema
      * is the cornerstone of applications/systems, so it's usually well defined, checked and tested
      * doesn't change as much as functionality, therefore there's a less chance of human error
      * is often used by multiple applications, so such an error will be noticed by a person sooner than an error in an application (in general)
  * DB queries
    * if using same queries/patterns for the app and the test; avoid this by using SeLite DataObjects and formulas to generate queries instead
    * if using SQLite for the app (not common for server-side web apps)
  * business/presentation logic - that's focus of SeLite
    * as a precaution, have corresponding parts of the app and the test implemented differently, e.g. developed
      * by different people, and/or
      * with a time period between implementing them, and/or
      * developing the test first and only then the app functionality, and/or
      * using different technique
        * this is implied for DB/business layer, since SeLite test components are in Javascript (while a server-side app is usually not in Javascript)
        * your test can use DbObjects extension (e.g. class DbRecordSetFormula) and write less custom SQL queries (no need to reinvent the wheel)

SeLite allows us to do this. See also [HandlingData](HandlingData).

# SQLite side effects of SeLite #
SQLite follows SQL standard pretty well, but it may differ to your application's RDBMS. That can cause false negative errors. See [SQLiteSpecifics](SQLiteSpecifics).