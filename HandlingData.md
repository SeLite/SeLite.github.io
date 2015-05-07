---
title: Handling data
layout: default
---

# Summary #
This explains how a web app and its test can operate with separate databases ([TestMethodsTheory](TestMethodsTheory) > [Web app and its test have separate DBs](TestMethodsTheory#3-web-app-and-its-test-have-separate-dbs)).

# Test data lifecycle #
Initially, the test database is a copy of the web application's DB (possibly with less tables and columns or records). For each test step, the test system triggers an operation in the browser and it also updates its own test DB to reflect that operation. The test DB updates don't need to be exactly same as app DB updates. The test only updates columns/tables that we use for validation. The test compares the result page of the web app to the updated test data.

If the app or the test fails, the test and web app database can get out-of-sync. That causes nonobvious knock-on errors later. That should normally happen only if

  * the test was incorrect (likely in the phase when you design tests), or
  * the web application is incorrect (this is what you want to detect by testing), or
  * there are errors outside both the test system and the web app (e.g. connectivity/infrastructure down...)

# Re-loading the data #
If the test and web app data is out of sync, you we want to re-load it. SeLite doesn't automate this. If we (ever) automate it, then we want it to

  * periodically take synchronised snapshots of test and app DB
  * back up the test state (e.g. Selenium variables), both the test and web app DB and take a screenshot, so that you can identify the problem later

If the error was outside of both the app and test, and the test can detect that, it could

  * reload the app and the test DB (if needed - if the failed operation is likely to apply some incomplete modifications, or modifications different between the app and the test)
  * sleep (ideally for a period that increases exponentially on each re-try) and re-try (repeating this for up to a limited number of times).

Otherwise, the error is in the app or the test. Re-loading the data won't fix the test neither the application. But the tester may need to test other parts before the failed part (of the app or the test) gets fixed. For that the tester needs to skip the failed test in the meantime.

We can re-load

  * just the test DB
    * to make it reflect the current web app DB
    * that only works if
      * the test was incorrect just under a specific condition
      * we fixed the test
      * the web app itself is correct
      * there were no errors from the third group (so the app DB is healthy)
      * the workflow of the web app allows us to re-run the test without re-loading the application DB. See below.
  * or: both the test DB and web app DB

There will also be a need to reload both DBs, even if everything is healthy (app and test OK and both DBs in sync). That's when testing a tool reached its designed limits. For example, the app may limit the number of submissions per user per day without a way to remove them through the application's web interface.

# Loose test data #
Some fields are difficult to replicate from the app DB to the test DB during the tests. Those are

  * IDs fields of inserted records
  * non-trivial references
  * create/update/delete timestamps

## IDs of new records ##
A lot of the following notes are for apps using Postgres for its DB. However, some notes and most of the ideas are not Postgres-specific.

Say you want your test to navigate by

  * IDs of new records inserted during tests, or
  * links reflecting IDs of those new records

That is needed e.g. for security testing to ensure that a user can't access record(s) created by other user(s). In such a case the test can't navigate through the web app to the forbidden page, so it needs to know the ID value to generate the link.

When the test creates a new record in its DB, it needs to set its ID to reflect the ID in the app DB. Methods:

  * **Generate/assume**
    * the last used values are the same between the test DB (SQLite) and the app DB, and the test DB (SQLite) uses same method to generate the new IDs as the app DB
    * this is optimistic - it breaks if one side gets broken temporarily and the last used values get out of sync
    * test SQLite tables can use following options (as relevant to SeLite)
      * INTEGER PRIMARY KEY, which
        * generates  an ID one larger than the largest ID currently present (unless the IDs reach 64bit signed limit)
        * reuses IDs of deleted rows (but only if they had the highest IDs, i.e. there are no existing rows with a higher ID)
      * INTEGER PRIMARY KEY AUTOINCREMENT
        * it generates an ID one larger than any ID ever present
        * it won't generate a new ID once there has been an ID reaching 64bit signed limit (OK in real world)
        * it doesn't re-use IDs of deleted rows
        * see SQLite documentation of [Autoincrement](http://www.sqlite.org/autoinc.html)
        * the import filter can set set next values of SQLite INTEGER PRIMARY KEY AUTOINCREMENT columns in special table [sqlite_sequence](http://www.sqlite.org/fileformat2.html#seqtab) (which is present only after there is at least one table with INTEGER PRIMARY KEY AUTOINCREMENT column).
      * both options reuse IDs from rolled back transaction
      * (both options require the key to be INTEGER, not INT)
    * app DB-specific
      * **PostgreSQL sequences**
        * if a transaction dies/is rolled back, it increases the sequence anyway, and any next transaction will use the next value. That's incompatible with SQLite INTEGER PRIMARY KEY and SQLite INTEGER PRIMARY KEY AUTOINCREMENT
        * in the initial app DB, last values of all (relevant) ID sequences must reflect the highest IDs in the respective tables. I.e. either
          * the last created record in each table still exists and it didn't get deleted. Otherwise Postgres sequence would use a subsequent ID for the next new record, while SQLite would use the ID of the deleted record (since the data dump doesn't contain the deleted record, and SQLite doesn't know that it has existed in past), or
          * when/just after the data dump from Postgres to SQLite, reset Postgres sequences to return the next available id for each relevant table. E.g. SELECT setval('your\_table\_id\_seq', (SELECT MAX(id) FROM your\_table)+1);
        * **simplified approach**
          * on app success:The test creates new SQLite record(s) and commits transaction(s), only after it detects that the web app processed the request successfully.
          * on app failure: If the web app failed and it didn't commit the INSERT transaction, it would increase the sequence. The test doesn't know that, its next available ID in that table will get out of sync, and next time the app & test insert a new record to the same table (in their respective DBs) the test will have knock on errors
        * **ideal approach (not easy)**
          * **on app success**
            * test creates a new record (commiting the transaction)
          * **on failure where the app starts an INSERT within a transaction, but it doesn't commit**
            * test could mark the affected table as 'unsure'; then the next time there is a successful insert by the app, it captures its ID and compares it
            * test creates a new record (commiting the transaction), then it deletes the new record
            * this app failure is not easy to differentiate from the following one
          * **on app failure without an INSERT**
            * detected e.g. as a session/single-sign out timeout, app DB down etc.
            * test doesn't create a new record
        * if the web app is not tied to Postgres, an alternative is to export the app data into a different DB that doesn't increase sequences by uncommitted transactions and run the app against that different DB
          * the simplest situation is if the web app can work with SQLite; then the DB import process means just copying the file
          * differences between RDBMS may apply. See [TestMethodsTheory](TestMethodsTheory) > [SQLite side effects of SeLite](TestMethodsTheory#sqlite-side-effects-of-SeLite) and [SQLiteSpecifics](SQLiteSpecifics).
      * other types of DB may need to reset their sequences
  * **Capture**
    * after submitting it shows a view page of the new submission. Its URL contains the new ID. Selenium IDE can extract it and save it in the test DB.
    * otherwise (if the application/module doesn't present the view page after submission) the test
      * navigates the application to a page that lists the records
      * navigates the application to sort those submissions so that the latest is shown first
      * identifies a link to the first submission on the list (XPath is good for that)
      * if the link is nice (i.e. it contains the ID of the submission)
        * extracts the ID from the link
      * or (e.g. the link is complex or a Javascript link, or it doesn't contain the ID in an obvious form)
        * navigate to the linked page to show the submission
        * extract the ID somehow
      * otherwise: use timestamp-based and list item position-based navigation and comparison
    * useful for testing security: the test logs on to the tested application as the other user (security victim), gets a listing of the forbidden records, captures id/link of the record(s) and stores it (in the test session or its DB). Then it logs out, logs in as the other user (security intruder) and tries to access the stolen URL(s).

## Non-trivial references ##
Non-trivial references are based on

  * random-based values, or
  * system-specific values not replicable in the test (e.g. univeraly unique IDs), or
  * data/configuration values not represented in the test DB.

If you want to validate them, you need to make the test capture those values and store them in the test DB whenever they get created/updated in the app.

# Reloading databases and creating/updating user passwords #
See [SettingsInterface](SettingsInterface) > [Reloading databases](SettingsInterface#reloading-databases). If your tests create/update user passwords, see [GeneralFramework](GeneralFramework) > [Preserving special values in test DB](GeneralFramework#preserving-special-values-in-test-db).

# External data #
Applications often use data sources outside of their DB (e.g. web services). If

  * the external data source is used read-only by the app and
  * there is no direct/indirect write flow from the application to that data and
  * the test can access the data

then have the test use that data (via a back door). That's fairly easy between SeLite and webservices. However, if the data source is a of some other type, then you'll need either (in order of complexity)

  * a webservice adapter on the server-side, or
  * a plugin/extension for Firefox (possibly a binary XPCOM component), or
  * custom-compiled Firefox with a target data source client and a Core extension of Selenium IDE in Javascript

If the app writes to the external data source (directly or indirectly), then treat it like another app DB. Have your test use (read and update) a writable copy of that external data (transformed to SQLite). You'll need script(s) to populate those table(s), so you can change and re-load the copy in future.