---
id: collect-261121-veille-ia/langages-dev/r-news-for-package-rcppsimdjson
title: "R: News for Package 'RcppSimdJson'"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-09"]
keywords: ["version", "dirk", "changes", "simdjson", "upgraded", "closing", "package", "daniel"]
source: https://stat.ethz.ch/CRAN/web/packages/RcppSimdJson/news.html
source_anchor: ""
source_lines: [1, 156]
section: "Langages_Dev"
---
| NEWS | R Documentation | 

## News for Package RcppSimdJson

### Changes in version 0.1.17 [unreleased] (2026-10-xx)

- Compilation starts with C++20 feature test; if this fails C++17 is used (Dirk in #111 fixing #110)
-  simdjson was upgraded to version 5.0.5 (Daniel in #112)

### Changes in version 0.1.16 (2026-10-09)

-  simdjson was upgraded to versions 4.6.1 and 4.6.3 (Dirk in #100 and #102 fixing #99 and #101)
-  simdjson was upgraded to version 5.0.2 (Daniel in #104)
- A number of small tweaks to the Rcpp interface were made too (Daniel in #104)
- Diagnostics for `gcc` and`clang` are not suppressed per
CRAN Policy (Dirk in #106 closing #105)
- The continuous integration matrix was extended to `g++` version 15 and 16 (Dirk in #108 closing #107)
- A few minor packaging nags were address (Dirk in #109)

### Changes in version 0.1.15 (2026-01-14)

-  simdjson was upgraded to version 4.2.4 (Dirk in #97
-  `RcppExports.cpp` was regenerated to aid a Rcpp
transition
- Standard maintenance updates for continuous integration and URLs

### Changes in version 0.1.14 (2025-09-13)

-  simdjson was upgraded to version 4.0.0 (Dirk in #96
- Continuous integration now relies a token for codecov.io

### Changes in version 0.1.13 (2025-03-07)

- A call to `std::string::erase` is now guarded (Daniel)
- The package now uses Authors@R as mandated by CRAN (Dirk)
-  simdjson was upgraded to version 3.12.2 (Dirk)
- Continuous integration updated to more compilers and simpler setup
- Two demos are now in `inst/examples` to not inflate Suggests

### Changes in version 0.1.12 (2024-07-05)

- Updated benchmarks now include 'yyjsonr'
-  simdjson was upgraded to version 3.9.5 (Daniel in #92 fixing #91)
- Additional small update for C++20 compilation under g++-14

### Changes in version 0.1.11 (2023-11-28)

-  `RcppExports.cpp` has been regenerated under an updated
Rcpp to address a`printf` format warning (Dirk in #88).

### Changes in version 0.1.10 (2023-05-14)

-  simdjson was upgraded to version 3.1.8 (Dirk in #85).

### Changes in version 0.1.9 (2023-01-21)

- The internal function `deseralize_json` is now exported at the C++
level as well as in R (Dirk in #81 closing #80).
-  simdjson was upgraded to version 3.0.1 (Dirk in #83).
- The package now defaults to C++17 compilation; `configure` has
been retired (Dirk closing #82).
- The three main R access functions now use a more compact argument check via `stopifnot` (Dirk).

### Changes in version 0.1.8 (2022-10-18)

- Use the '||' operator instead of '|' on a set of booleans to appease 'clang-14'.

### Changes in version 0.1.7 (2022-02-18)

- Two URLs were updated in 'README.md', and Travis artifacts and badges have been removed (Dirk).
- One unit test file was updated to not trigger a 'LENGTH_1' warning (Dirk closing #76).

### Changes in version 0.1.6 (2021-09-07)

- The C++17 dependency was stated more clearly in the DESCRIPTION file (Dirk)
- The simdjson version was updated to release 1.0.0 (Daniel Lemire in #70).

### Changes in version 0.1.5 (2021-02-23)

- Temporary filenames from request URLs need an extra path sanitization on Windows as seen with package td.
- A few `#nocov` tags were added to the code.

### Changes in version 0.1.4 (2021-02-12)

- Support additional headers in `fload` (Dirk in #60).
- Enable continuous integration via GitHub Actions using `run.sh` from r-ci repo (Dirk in #61, #62).
- Add option to always return list to `fparse()` /`fload()` (Brendan in #65 closing #64).

### Changes in version 0.1.3 (2020-11-01)

- Added URLs to DESCRIPTION (Dirk closing #50).
- Upgraded to simdjson 0.6.0 (Dirk in #52).
- New policy option to always convert integers to `int64_t` (Brendan in #55 closing #54).
- Added workaround for odd clang-9 bug (Brendan in #57).
- New utility functions `is_valid_utf8()` ,`is_valid_json()` and`fminify()` (Brendan in #58).

### Changes in version 0.1.2 (2020-10-07)

- Upgraded to simdjson 0.5.0 (Dirk in #49).

### Changes in version 0.1.1 (2020-08-10)

- Corrected incorrect file deletion when mixing local and remote files (Brendan in #34) closing #33.
- Added support for raw vectors, compressed files, and compressed downloads (Dirk and Brendan in #36, #39, and #45 closing #35 and addressing issues raised in #40 and #44).
- Examples in two demos are now more self-sufficient (Daniel Lemire and Dirk in #42).
- Expanded query functionality to include single, flat, and nested queries (Brendan in #45 closing #43).
- Split error handling parameters from `error_ok` /`on_error` into`parse_error_ok` /`on_parse_error` and`query_error_ok` /`on_query_error` (Brendan in #45).
- One-line upstream change to address sanitizer error on cast.

### Changes in version 0.1.0 (2020-07-07)

- Upgraded to simdjson 0.4.1 which adds upstream Windows support (Dirk in #27 closing #26 and #14, plus extensive work by Brendan helping upstream with mingw tests).
- Upgraded to simdjson 0.4.6 with further upstream improvements (Dirk in #30).
- Change Travis CI to build matrix over g++ 7, 8, 9, and 10 (Dirk in #31; and also Brendan in #32).
- New JSON functions `fparse` and`fload` (Brendan in #32) closing #18 and #10).

### Changes in version 0.0.6 (2020-06-25)

- Created C++ integer-handling utilities for safe downcasting and integer return (Brendan in #16 closing #13).
- New JSON functions `.deserialize_json` and`.load_json` (Brendan in #16, #17, #20, #21).
- Upgrade Travis CI to 'bionic', extract package and version from `DESCRIPTION` (Dirk in #23).
- Upgraded to simdjson 0.4.0 (Dirk in #25 closing #24).

### Changes in version 0.0.5 (2020-05-23)

- Add `parseExample` from earlier upstream announcement (Dirk).
- Synced with upstream (Brendan in #12) closing #11).
- Updated example `parseExample` to API changes (Brendan).

### Changes in version 0.0.4 (2020-04-03)

- Upgraded to new upstream releases 0.3 and 0.3.1 (Dirk in #9 closing #8)
- Updated example `validateJSON` to API changes.

### Changes in version 0.0.3 (2020-03-03)

- Sychronized once more with upstream.
- Created new C++ function to check for unsupported architecture, and report the status on package load.
- Only run example and unit tests if supported architecture is found.
- Created small `configure` script to see if R was built
with C++17 support, and record it in`src/Makevars` .

### Changes in version 0.0.2 (2020-02-21)

- Sychronized with upstream (Dirk in #4 and #5).
- The R side of `validateJSON` now globs the file argument,
expanding symbols like`~` appropriately.
- C++ code in `validateJSON` now conditional on C++17
allowing (incomplete) compilation on lesser systems.
- New helper function returning value of `__cplusplus` macro, used in package startup to warn if insufficient compiler used.

### Changes in version 0.0.1 (2020-01-24)

- Initial CRAN upload of first version
- Comment-out use of `stdout` (now updated upstream)
- Deactivate use computed GOTOs for compiler compliance and CRAN Policy via `#define`
