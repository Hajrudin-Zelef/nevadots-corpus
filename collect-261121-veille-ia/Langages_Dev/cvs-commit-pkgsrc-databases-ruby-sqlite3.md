---
id: collect-261121-veille-ia/langages-dev/cvs-commit-pkgsrc-databases-ruby-sqlite3
title: "CVS commit: pkgsrc/databases/ruby-sqlite3"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-28"]
keywords: ["database", "sqlite", "each", "statement", "pkgsrc", "databases", "ruby-sqlite3", "flavorjones"]
source: https://mail-index.NetBSD.org/pkgsrc-changes/2026/10/28/msg358293.html
source_anchor: ""
source_lines: [1, 172]
section: "Langages_Dev"
---
pkgsrc-Changes archive

[Date Prev][Date Next][Thread Prev][Thread Next][Date Index][Thread Index][Old Index]
# CVS commit: pkgsrc/databases/ruby-sqlite3



```
Module Name:    pkgsrc
Committed By:   taca
Date:           Wed Oct 28 15:21:13 UTC 2026
Modified Files:
        pkgsrc/databases/ruby-sqlite3: Makefile distinfo
Log Message:
databases/ruby-sqlite3: update to 2.10.2
2.10.0 (2026-10-13)
Dependencies
* Vendored sqlite is updated to v3.54.0 (from v3.53.2). #750 @flavorjones
* Notable changes in this version include:
    - ALTER TABLE now raises an error when adding, renaming, or dropping a
      column named ROWID, _ROWID_, or OID.
    - The authorizer callback is now invoked for SQL functions in the
      DEFAULT clauses of CREATE TABLE statements.
    - SQLite no longer supports Windows XP or earlier.  On Windows, the
      precompiled gem and any installation that compiles the vendored sqlite
      now require Windows Vista or later.
Added
* ResultSet#each and #each_hash return an Enumerator when called without a
  block. Previously they raised LocalJumpError. #656 @kch
* Introduce Database#serialize, which returns a database as a binary string,
  and Database#deserialize, which replaces a database with the contents of
  such a string.  These wrap sqlite3_serialize() and sqlite3_deserialize().
  Both methods are defined only when SQLite was compiled without
  SQLITE_OMIT_DESERIALIZE, and Database#deserialize also requires that the
  loaded SQLite is v3.54.0 or later.  An image of a database in WAL mode
  cannot be queried after deserializing; see the Database#deserialize
  documentation.  #685 @Maaarcocr
Fixed
* Fix Database#statement_timeout= interrupting statements almost immediately
  instead of after the timeout elapsed, including re-executed prepared
  statements and calls to Database#execute_batch2.  #737 @palkan
  @flavorjones
Improved
* On CRuby, Statement#types allocates fewer strings.  Each declared type is
  downcased and interned in C, instead of allocating two strings per column.
  #743 @jhawthorn
Changed
* ResultSet#each and #each_hash return the result set when called with a
  block.  As a consequence, Database#execute and #execute2 called with a
  block return the closed ResultSet instead of nil.  #656 @kch
* Statement#columns and Statement#types return frozen arrays, and the
  strings in Statement#types are frozen.  Code that mutates them now raises
  FrozenError.  #743 @jhawthorn
* Statement#column_decltype and Statement#types return UTF-8 strings.
  Previously they returned ASCII-8BIT strings.  #743 @jhawthorn
2.10.1 (2026-10-19)
Dependencies
* Vendored sqlite is updated to v3.54.1 (from v3.54.0).  This release
  contains a fix for a possible heap buffer overflow on 32-bit builds,
  including the precompiled x86-linux-gnu and x86-linux-musl gems.  #756
  @flavorjones
Improved
* Database#define_aggregator no longer creates an anonymous class for each
  aggregate function it defines.  Each invocation of the function still runs
  on a clone of the template object.  #752 @jhawthorn
2.10.2 (2026-10-26)
Security / Stability
* Raise RangeError when a SQL string, a bound String or Blob value, or a
  String or Blob returned from a custom function or aggregate is longer than
  INT_MAX bytes.  Previously the length was truncated to 32 bits, so SQLite
  could read only part of the string and drop trailing clauses from a query.
  See GHSA-4fqr-96q8-xrfx for more information.  #762 @flavorjones
* Fix a crash when a custom aggregate's finalize returns a value that can't
  be converted to a SQLite result.  The exception is now raised from the
  query and the connection remains usable.  #762 @flavorjones
* Fix a use-after-free when the hash returned by Database#collations is
  cleared, modified or frozen.  GC could free a comparator that SQLite still
  called.  When that hash is frozen, Database#collation still raises
  FrozenError, but the new comparator stays registered and in use.  #764
  @flavorjones
* Fix a deadlock when a collation comparator, trace block, busy_handler
  block or authorizer raises, or when an interrupt such as Thread#raise
  arrives while an aggregate's handler is being created.  Another thread
  that used the connection blocked forever.  The exception is now raised
  from the query and the connection remains usable.  See "Changed" below for
  what SQLite does before the exception is raised.  #764 @flavorjones
* Fix a crash when a custom function or aggregate uses throw.  The throw now
  unwinds to its catch.  #764 @flavorjones
Changed
* When a callback raises or throws, SQLite now finishes its current call
  before the exception is raised, instead of being unwound in the middle of
  it.  #764 @flavorjones
    - When a trace block raises, SQLite runs the statement up to its first
      row, or to completion, before the exception is raised.  Previously the
      statement stopped where the block raised.  For example, an INSERT
      whose trace block raises now inserts its row.
    - The other callbacks in that SQLite call skip their Ruby code:
      collation comparators return 0, functions and aggregates report an
      error to SQLite, the busy handler stops retrying and the authorizer
      denies.
    - Database#execute_batch2 now raises the exception from a custom
      function, instead of SQLite3::SQLException "Ruby Exception occurred".
    - An exception from a callback that SQLite runs during Statement#reset!
      or Database#close is discarded.
* The extension keeps per-fiber state in
  Thread.current[:__sqlite3_callback_frames__].  A query raises FrozenError
  on a Thread that was frozen before its first query.  #764 @flavorjones
* When SQLite can't allocate an aggregate's context, the query raises
  SQLite3::MemoryException instead of aborting the process.  #764
  @flavorjones
Improved
* Statement#step no longer allocates and discards an array when the
  statement has no more rows.  This saves one object per statement
  execution.  #758 @thomasklemm
* ObjectSpace.dump and heap profilers now report SQLite3::Database and
  SQLite3::Statement objects under their own type names, instead of
  SQLite3::Backup.  #764 @flavorjones
Documentation
* Document that parameters bound by Statement#execute, #execute!,
  #bind_param and #bind_params stay bound across executions, and that
  Statement#clear_bindings! clears them.  #751 @flavorjones
To generate a diff of this commit:
cvs rdiff -u -r1.101 -r1.102 pkgsrc/databases/ruby-sqlite3/Makefile
cvs rdiff -u -r1.62 -r1.63 pkgsrc/databases/ruby-sqlite3/distinfo
Please note that diffs are not public domain; they are subject to the
copyright notices on the relevant files.
```
```
Modified files:
Index: pkgsrc/databases/ruby-sqlite3/Makefile
diff -u pkgsrc/databases/ruby-sqlite3/Makefile:1.101 pkgsrc/databases/ruby-sqlite3/Makefile:1.102
--- pkgsrc/databases/ruby-sqlite3/Makefile:1.101        Wed Aug 12 04:44:16 2026
+++ pkgsrc/databases/ruby-sqlite3/Makefile      Wed Oct 28 15:21:13 2026
@@ -1,6 +1,6 @@
-# $NetBSD: Makefile,v 1.101 2026/08/12 04:44:16 taca Exp $
+# $NetBSD: Makefile,v 1.102 2026/10/28 15:21:13 taca Exp $
 
-DISTNAME=      sqlite3-2.9.6
+DISTNAME=      sqlite3-2.10.2
 CATEGORIES=    databases
 
 MAINTAINER=    pkgsrc-users%NetBSD.org@localhost
@@ -14,7 +14,7 @@ USE_GCC_RUNTIME=      yes
 RUBY_EXTCONF_ARGS=     --enable-system-libraries
 OVERRIDE_GEMSPEC+=     :files ${BUNDLED_ARCHIVE}=
 
-BUNDLED_ARCHIVE=       ports/archives/sqlite-autoconf-3530200.tar.gz
+BUNDLED_ARCHIVE=       ports/archives/sqlite-autoconf-3540100.tar.gz
 
 pre-configure:
        ${RM} -f ${WRKSRC}/${BUNDLED_ARCHIVE}
Index: pkgsrc/databases/ruby-sqlite3/distinfo
diff -u pkgsrc/databases/ruby-sqlite3/distinfo:1.62 pkgsrc/databases/ruby-sqlite3/distinfo:1.63
--- pkgsrc/databases/ruby-sqlite3/distinfo:1.62 Wed Aug 12 04:44:16 2026
+++ pkgsrc/databases/ruby-sqlite3/distinfo      Wed Oct 28 15:21:13 2026
@@ -1,5 +1,5 @@
-$NetBSD: distinfo,v 1.62 2026/08/12 04:44:16 taca Exp $
+$NetBSD: distinfo,v 1.63 2026/10/28 15:21:13 taca Exp $
 
-BLAKE2s (sqlite3-2.9.6.gem) = f7782509ae506f8c171e039946a0721d634f55db46fbdc82b857ff37f94f7075
-SHA512 (sqlite3-2.9.6.gem) = 817226e0b1bfa5e044d856e692ca2e8d959de6d079cc8b311727cf95c513877b8bb9b0d4354993f8e284fb5eda1411f32203019a8d83f2d15a23794d65271a4a
-Size (sqlite3-2.9.6.gem) = 3350016 bytes
+BLAKE2s (sqlite3-2.10.2.gem) = d8c8ed1a728390b44235b83897ab6565e267abb28f9851c7db05b20827d8381d
+SHA512 (sqlite3-2.10.2.gem) = 7316fe1eeab7f9b8ea4b867f300d29810788eaa7b833597af685b5431f44d253a63302e01a0cb63ab0a713b4615ad6da11efc6045ee1079df0f90f7dd7e8db4c
+Size (sqlite3-2.10.2.gem) = 3410944 bytes
```


**Home |
Main Index |
Thread Index |
Old Index**
