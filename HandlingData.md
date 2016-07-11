---
layout: default
---
{% include links %}
* TOC
{:toc}

# Summary #
This explains how a web app and Selenese [scripts][script] can operate with separate databases ([TestMethodsTheory](TestMethodsTheory) > [Web app and its test have separate DBs](TestMethodsTheory#web-app-and-its-test-have-separate-dbs)).

# Script data lifecycle
Initially, the [script DB] is a copy of the web application's DB (possibly with less tables and columns or records). For each logical step, the [script] triggers an operation in the browser and it also updates its own DB to reflect that operation. Updates in [script DB] don't need to be exactly same as updates in [app DB]. The script only updates columns/tables that we use for validation. The script compares the result page of the web app to the updated script data.

If the app or the script fails, their databases can get out-of-sync. That causes non-obvious knock-on errors later. That should normally happen only if

  * the script was incorrect (likely in the design phase), or
  * the web application is incorrect (this is what you want to detect by testing), or
  * there are errors outside both the script system and the web app (e.g. connectivity/infrastructure down...)

# Re-loading the data #
If the script and web app data is out of sync, you we want to re-load it. SeLite doesn't automate this. If we (ever) automate it, then we want it to

  * periodically take synchronised snapshots of script and [app DB]
  * back up the script state (e.g. Selenium variables), both script and web [app DB] and take a screenshot, so that you can identify the problem later

If the error was outside of both the app and the script, and the script can detect that, it could

  * reload [app DB] and [script DB] (if needed - if the failed operation is likely to apply some incomplete modifications, or modifications different between the app and the script)
  * sleep (ideally for a period that increases exponentially on each re-try) and re-try (repeating this for up to a limited number of times).

Otherwise, the error is in the app or in the script. Re-loading the data won't fix the script neither the application. It is beneficial, however, when user needs to use other parts (before the failed part, whether of the app or the script, gets fixed).

We can re-load

  * just the [script DB]
    * to make it reflect the current web [app DB]
    * that only works if
      * the script was incorrect just under a specific condition
      * we fixed the script
      * the web app itself is correct
      * there were no errors from the third group (so the [app DB] is healthy)
      * the workflow of the web app allows us to re-run the script without re-loading the [app DB]. See below.
  * or: both [script DB] and [app DB]

There will also be a need to reload both DBs, even if everything is healthy (app and [script] are OK and both DBs in sync). That's when we reach designed limits of the application. For example, the app may limit the number of submissions per user per day without a way to remove them through the application's web interface.

# Loose script data
Some fields are difficult to replicate from [app DB] to [script DB] during the runs. Those are

  * IDs fields of inserted records
  * non-trivial references
  * create/update/delete timestamps

## IDs of new records ##
Many of the following notes are for Postgres or SQLite. However, some notes and most of the ideas are not DB-specific.

Navigating by record IDs is needed e.g. for security testing to ensure that a user can't get inappropriate access. In such a case the script can't navigate through the web app to the forbidden page, so it needs to know the ID value to generate the link (to verify that the access is denied).

When the script creates a new record in its DB, it needs to set its ID to reflect the ID in the [app DB]. Methods:

  * **Generate/assume**
    * the last used values are the same between [script DB] (SQLite) and [app DB], and [script DB] (SQLite) uses same method to generate the new IDs as [app DB]
    * this is optimistic - it breaks if one side gets broken temporarily and the last used values get out of sync
    * script SQLite tables can use following options (as relevant to SeLite)
      * `INTEGER PRIMARY KEY`, which
        * generates  an ID one larger than the largest ID currently present (unless the IDs reach 64bit signed limit)
        * reuses IDs of deleted rows (but only if they had the highest IDs, i.e. there are no existing rows with a higher ID)
      * `INTEGER PRIMARY KEY AUTOINCREMENT`
        * it generates an ID one larger than any ID ever present
        * it won't generate a new ID once there has been an ID reaching 64bit signed limit (OK in real world)
        * it doesn't re-use IDs of deleted rows
        * see SQLite documentation of [Autoincrement](http://www.sqlite.org/autoinc.html)
        * the import filter can set set next values of SQLite `INTEGER PRIMARY KEY AUTOINCREMENT` columns in special table [sqlite_sequence](http://www.sqlite.org/fileformat2.html#seqtab) (which is present only after there is at least one table with `INTEGER PRIMARY KEY AUTOINCREMENT` column).
      * both options reuse IDs from rolled back transaction
      * (both options require the key to be `INTEGER`, not `INT`)
    * [app DB]-specific
      * **PostgreSQL sequences**
        * if a transaction dies/is rolled back, it increases the sequence anyway, and any next transaction will use the next value. That's incompatible with SQLite `INTEGER PRIMARY KEY` and SQLite `INTEGER PRIMARY KEY AUTOINCREMENT`.
        * when importing/loading [app DB], last values of all (relevant) ID sequences must reflect the highest IDs in the respective tables. I.e. either
          * the last created record in each table still exists and it didn't get deleted. Otherwise Postgres sequence would use a subsequent ID for the next new record, while SQLite would use the ID of the deleted record (since the data dump doesn't contain the deleted record, and SQLite doesn't know that it has existed in past), or
          * when/just after the data dump from Postgres to SQLite, reset Postgres sequences to return the next available id for each relevant table. E.g. `SELECT setval('your_table_id_seq', (SELECT MAX(id) FROM your_table)+1)`
        * **simplified approach**
          * on app success: The script creates new SQLite record(s) and commits transaction(s), only after it detects that the web app processed the request successfully.
          * on app failure: If the web app failed and it didn't commit the `INSERT `transaction, it would increase the sequence. The script doesn't know that, its next available ID in that table will get out of sync, and next time the app & script insert a new record to the same table (in their respective DBs) the script will have knock on errors
        * **ideal approach (not easy)**
          * **on app success**
            * script creates a new record (commiting the transaction)
          * **on failure where the app starts an `INSERT` within a transaction, but it doesn't commit**
            * script could mark the affected table as _unsure_; then the next time there is a successful insert by the app, it captures its ID and compares it
            * script creates a new record (commiting the transaction), then it deletes the new record
            * this app failure is not easy to differentiate from the following one
          * **on app failure without an `INSERT`**
            * detected e.g. as a session/single-sign out timeout, [app DB] down etc.
            * script doesn't create a new record
        * if the web app is not tied to Postgres, an alternative is to export the app data into a different DB that doesn't increase sequences by uncommitted transactions and run the app against that different DB
          * the simplest situation is if the web app can work with SQLite; then the DB import process means just copying the file
          * differences between RDBMS may apply. See [TestMethodsTheory](TestMethodsTheory) > [SQLite side effects of SeLite](TestMethodsTheory#sqlite-side-effects-of-SeLite) and [SQLiteSpecifics](SQLiteSpecifics).
      * other types of DB may need to reset their sequences
  * **Capture**
    * after submitting it shows a view page of the new submission. Its URL contains the new ID. Selenium IDE can extract it and save it in [script DB].
    * otherwise (if the application/module doesn't present the view page after submission) the script
      * navigates the application to a page that lists the records
      * navigates the application to sort those submissions so that the latest is shown first
      * identifies a link to the first submission on the list (XPath is good for that)
      * if the link is nice (i.e. it contains the ID of the submission)
        * extracts the ID from the link
      * or (e.g. the link is complex or a Javascript link, or it doesn't contain the ID in an obvious form)
        * navigate to the linked page to show the submission
        * extract the ID somehow
      * otherwise: use timestamp-based and list item position-based navigation and comparison
    * useful for testing security: the script logs on to the application as the other user (security victim), gets a listing of the forbidden records, captures id/link of the record(s) and stores it (in the script session or its DB). Then it logs out, logs in as the other user (security intruder) and tries to access the stolen URL(s).

## Non-trivial references ##
Non-trivial references are based on

  * random-based values, or
  * system-specific values not replicable in the script (e.g. univeraly unique IDs), or
  * data/configuration values not represented in [script DB].

If you want to validate them, you need to make the script capture those values and store them in [script DB] whenever they get created/updated in the app.

# Reloading databases and creating/updating user passwords #
See {{navReloadingDatabases}}. If your scripts create/update user passwords, see {{navPreservingSpecialValuesInScriptDb}}.

# External data #
Applications often use data sources outside of their DB (e.g. web services). If

  * the external data source is used read-only by the app and
  * there is no direct/indirect write flow from the application to that data and
  * the [script] can access the data

then have the [script] use that data (via a back door). That's fairly easy between SeLite and webservices. However, if the data source is a of some other type, then you'll need either (in order of complexity)

  * a webservice adapter on the server-side, or
  * a plugin/extension for Firefox (possibly a binary XPCOM component), or
  * custom-compiled Firefox with a target data source client and a [Core extension] of Selenium IDE in Javascript

If the app writes to the external data source (directly or indirectly), then treat it like another [app DB]. Have your [script] use (read and update) a writable copy of that external data (transformed to SQLite). You'll need script(s) to populate those table(s), so you can change and re-load the copy in future.

# No column name _toString_ #
You must not have a DB column with name `toString`. That's because Javascript objects representing DB records have method `toString()` for convenience.