---
title: SQLite import
layout: default
---

## Overview ##
This converts a backup of your database to SQLite. It does not and it will not connect to your DB.

For now
  * the JAR is not distributed. Compile yourself or contact me.
  * the imports only work from Postgres
  * only some schema and insert SQL of Postgres was tested
  * TODO autoincrement primary keys

You can develop application-specific filters that only import data relevant to testing.

Following steps are in Unix. They're quite trivial, so there's no version for Windows here.

You need 2 plain text backups of Postgres - one with schema, the other one with data. Data backup must be created by pg\_dump tool, with one insert per record and including column names (as is the default).

MySQL (future): --no-data, --no-create-info

## Importing the schema ##
```
java -ea -jar SeliteFilter.jar
com.googlecode.selite.filter.apps.Moodle pg_structure-orig.sql schema.sql --usage schema

echo "PRAGMA synchronous = OFF; PRAGMA journal_mode= OFF; BEGIN TRANSACTION;" >schema-fast.sql
cat schema.sql >>schema-fast.sql
echo "END TRANSACTION;" >>schema-fast.sql

rm -f schema.sqlite
sqlite3 schema.sqlite <schema-fast.sql
```

## Importing the data ##
```
# 'data' is the default value of 'usage' option
java -ea -jar SeliteFilter.jar com.googlecode.selite.filter.apps.Moodle pg_data-orig.sql data.sql

# see also http://www.sqlite.org/pragma.html#pragma_cache_size
echo "PRAGMA synchronous = OFF; PRAGMA journal_mode= OFF; BEGIN TRANSACTION;" >data-fast.sql
cat data.sql >>data-fast.sql
echo "END TRANSACTION;" >>data-fast.sql

cp schema.sqlite all.sqlite
sqlite3 all.sqlite <data-fast.sql

# all.sqlite is the result
```