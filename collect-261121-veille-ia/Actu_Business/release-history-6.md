---
id: collect-261121-veille-ia/actu-business/release-history-6
title: "Release History"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2000-06-01"]
keywords: ["added", "database", "release", "files", "support", "history", "access", "same"]
source: https://sqlite.org/draft/changes.html
source_anchor: ""
source_lines: [7501, 7537]
section: "Actu_Business"
---
1. All database files to be modified by an UPDATE, INSERT or DELETE are
now locked before any changes are made to any files.
This makes it safe (I think) to access
the same database simultaneously from multiple processes.
2. The code appears stable so we are now calling it "beta".

### 2000-06-01

1. Better support for file locking so that two or more processes
(or threads)
can access the same database simultaneously.  More work needed in
this area, though.

### 2000-05-31

1. Added support for aggregate functions (Ex: **COUNT(*)** ,**MIN(...)** )
to the SELECT statement.
2. Added support for **SELECT DISTINCT ...**

### 2000-05-30

1. Added the **LIKE** operator.
2. Added a **GLOB** operator: similar to**LIKE** but it uses Unix shell globbing wildcards instead of the '%'
and '_' wildcards of SQL.
3. Added the **COPY** command patterned after
PostgreSQL so that SQLite
can now read the output of the**pg_dump** database dump utility
of PostgreSQL.
4. Added a **VACUUM** command that calls the**gdbm_reorganize()** function on the underlying database
files.
5. And many, many bug fixes...

### 2000-05-29

1. Initial Public Release of Alpha code

*** DRAFT ***
