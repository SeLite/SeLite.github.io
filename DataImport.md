---
title: Data import
layout: default
---

# Using DB filters #
Does your app use RDBMS other than SQLite and would you like your tests to have a copy of it? Then you want to export the data from your app DB to SQLite. For that you need

  * [SQLite binary](http://www.sqlite.org/download.html)
    * 3.6.20 distributed with CentOS 6.2 x64 used to generate strange 'Error: file is encrypted or is not a database'. 3.7.16 compiled from source works well.
    * 3.7.17 distributed with Fedora 19
  * [Java JRE](http://www.java.com) 7+
    * you don't need any JDBC drivers
  * Java libraries
    * [Argparse4j](http://sourceforge.net/projects/argparse4j/files/latest/download?source=dlp)
    * Reflections [from GitHub](http://github.com/ronmamo/reflections) or [from maven.org](http://repo1.maven.org/maven2/org/reflections/reflections/)
    * you only need those on the system where you are importing the backup to be used with your tests; you don't need those when you develop/run the tests
    * you don't need to download these, if you use NetBeans or Maven. See below.
  * you don't need full JDK neither a Java editor/IDE, unless you want to create custom filters.

# Development of custom filters #
  * get `src/filter` from [Source](https://code.google.com/p/selite/source/checkout)
  * get Java SE JDK 7+
  * you may want to get [NetBeans IDE](http://www.netbeans.org)
    * get NetBeans for Java SE
    * Menu File > Open Project... > choose `src/filter/pom.xml`. That will open a Maven-based project and it will get dependencies automatically. Otherwise you need Argparse4j and Reflections as above.
    * if you also want to develop Javascript code modules or Selenium Core extensions, add PHP support to your NetBeans via menu Tools > Plugins.