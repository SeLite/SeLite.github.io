---
layout: default
---
{% include links %}
* TOC
{:toc}

# Operator LIKE and case sensitivity #
In SQLite, operator LIKE matches case-insensitively. By default that applies to ASCII characters only; see below for more details on Unicode.

# Sorting and case sensitivity #

Sorting of DB results varies between database systems, or even between various configurations. For example, PostgreSQL can use various collation types. Most (or all) UTF8-based ones sort strings case-insensitively. 'C' collaction sorts case-sensitively, but it doesn't support Unicode.

If you need to sort case-insensitively in SQLite, that works only for ASCII characters by default. You can do it at either

  * table definition level:<code><br/>
    CREATE TABLE items( item VARCHAR(255) COLLATE NOCASE);<br/>
    INSERT INTO items(item) VALUES ('a'), ('b'), ('B'), ('A');<br/><br/>
    SELECT * FROM items ORDER BY item;
    </code>
  * query level:<code><br/>
    CREATE TABLE items( item VARCHAR(255) );<br/>
    INSERT INTO items(item) VALUES ('a'), ('b'), ('B'), ('A');<br/><br/>
    SELECT * FROM items ORDER BY item COLLATE NOCASE; --for ASCII only<br/>
    SELECT * FROM items ORDER BY UPPER(item); -- for UTF, too - but only if you enable Unicode - read below
    </code>

If you need case-insensitive sorting and Unicode, see [SQLite FAQ on Unicode](http://www.sqlite.org/faq.html#q18), [SQLITE\_ENABLE\_ICU compilation option](http://www.sqlite.org/compile.html#enable_icu) and [SQLite ICU README](http://www.sqlite.org/src/artifact?ci=trunk&filename=ext/icu/README.txt). On CentOS 6.4 I've installed `icu` using Add/Remove Software, but I couldn't get it to work. If you figure it out, please let me know.

Another way around is to reconfigure DB of the web application being automated, so that [app DB] performs same sorting as SQLite. Since that modified DB configuration is not what you use in production, this is not 100%-proof testing, but nothing is anyway.

# No literals `true` and `false` #
SQLite allows `boolean` column type, but it doesn't have constants `true` and `false`. Using literals `true` and `false` (without apostrophes) in SQLite queries fails. However, if you use prepared statements in Firefox Javascript, those convert Javascript boolean `true` and `false` to 1 and 0, respectively.