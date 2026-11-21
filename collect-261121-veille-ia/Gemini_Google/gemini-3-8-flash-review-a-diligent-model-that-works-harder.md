---
id: collect-261121-veille-ia/gemini-google/gemini-3-8-flash-review-a-diligent-model-that-works-harder
title: "Gemini 3.8 Flash Review: A Diligent Model That Works Harder"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["flash", "model", "gemini", "google", "more", "user", "diligent", "review"]
source: https://developer.puter.com/blog/gemini-3-8-flash-review/
source_anchor: ""
source_lines: [1, 359]
section: "Gemini_Google"
---
# Gemini 3.8 Flash Review: A Diligent Model That Works Harder

Google's newest Flash-tier model, Gemini 3.8 Flash, is being pitched as a drop-in upgrade for anyone already on Gemini 3.7 Flash. We looked at the price and benchmark comparison, and tested the claim that the model is more diligent than its predecessor.

## Same Price, Better Benchmarks

According to Google's official Gemini API pricing page, Gemini 3.8 Flash and Gemini 3.7 Flash are both listed at $0.75 per million input tokens and $3.75 per million output tokens. There's no markup for the newer model, at least not during its introductory pricing window, which Google has said runs through the end of 2026.

On top of matching the price, Google reports meaningful gains on several benchmarks. The company says 3.8 Flash outperforms 3.7 Flash and a number of larger frontier models on DeepSWE v1.1, a long-horizon software engineering benchmark, as well as on Vals Finance Agent V2 and Harvey's Legal Agent Benchmark. Google also reports a score of 54.9% on HLE-Verified, a benchmark spanning multi-step reasoning across STEM, humanities, and professional fields.

## Why It's Better: Diligence, and What That Costs

Gemini 3.8 Flash is more diligent than 3.7 Flash, because on complex tasks it's built to take extra reasoning steps and call tools iteratively, according to Google's launch post. Google's developer documentation adds that the model also verifies its own work on difficult multi-step goals, and attributes part of the improvement to training in demanding domains and to long-running agentic loops that recursively evaluate and refine the model during development.

Some models answer in one pass and stop, even when that first answer is incomplete. A model that does this on a task with multiple issues will often catch the one that's easiest to spot and miss the rest. Google is saying 3.8 Flash is built to keep working past that first plausible answer, checking itself before it hands a response back.

Effort levels are configurable by developers, and more reasoning steps and more tool calls mean more output tokens for the same task. On a per-token pricing model that turns into more cost and more latency. The pricing being identical to 3.7 Flash doesn't mean a given task costs the same to run, since 3.8 Flash spending more tokens thinking through a problem can raise the total bill for that task even though the rate per token hasn't changed.

## Testing It

We built a small test aimed directly at the diligence claim, using the same pass/fail approach we used to test Gemini 3.7 Flash's instruction following. We gave the model a short Python function containing three separate bugs, and asked for a general code review without saying how many issues to look for.

```
def get_user_orders(user_id, db, limit=10, cache={}):
    if user_id in cache:
        return cache[user_id]
    query = f"SELECT * FROM orders WHERE user_id = {user_id} LIMIT {limit}"
    results = db.execute(query)
    total_spent = sum(r['amount'] for r in results) / len(results)
    cache[user_id] = results
    return results, total_spent
```
The three bugs were chosen because they need different kinds of attention:

- **SQL injection** in the f-string query is a well-known pattern that most models are heavily trained to recognize, so catching it alone doesn't say much.
- **The mutable default argument** (`cache={}` ) is harder. Python evaluates default arguments once, at function definition time, so this dictionary is silently shared across every call for the life of the process, and catching it means tracing execution across multiple calls rather than matching a keyword.
- **The division by zero** , when a user has no orders, isn't hinted at anywhere in the prompt. Catching it means imagining an edge case the code doesn't show you, not reading what's on the page.

A model that pattern-matches for the one obviously bad line and stops would likely flag the SQL injection and miss the other two. We ran the same prompt against Gemini 3.8 Flash three separate times and checked each response against all three bugs, plus anything else it found on its own.

## Result

- All three trials caught all three bugs, SQL injection, the mutable default argument, and the division by zero, and correctly fixed each one. We checked the corrected code by hand rather than trusting the model's own description of it.
- Every trial also caught problems beyond what we asked about: an inconsistent return type between the cache-hit and cache-miss paths, a cache key that ignores the `limit` parameter,`total_spent` actually computing an average instead of a total, and a cursor-exhaustion risk.
- Trial 1 added an unprompted `ValueError` guard on a negative`limit` . Trial 2 hedged rather than committing to one fix, offering both a cache-free version and a version with an explicit`cache=None` parameter.
- Response times varied more than the reasoning did. Trial 1 and trial 3 both came back in 18.6 seconds; trial 2 took 41.1 seconds for a similar depth of analysis, a spread that matches the token cost Google warns about.

Based on this test, the diligence claim holds up. The model didn't stop at the SQL injection, the bug most models are already trained to flag. It also caught the mutable default argument and the division by zero, the two bugs that needed tracing execution or imagining an edge case rather than pattern-matching a keyword, and it kept going past all three to flag four more problems nobody asked it to look for.

What this test can't show is a comparison. We didn't run the same prompt against a model that stops early, so we can't say how much of this is specific to 3.8 Flash versus something a capable model would already do on a code review this size.

## Full Trial Responses

All three responses are below, unedited, exactly as the model returned them.

**Trial 1 (18.6 seconds)**

Reasoning trace:

```
Reviewing User Orders
I'm currently examining the get_user_orders function, looking to ensure its correctness and safety. My focus is on identifying and rectifying any potential issues within the implementation.
Addressing Code Vulnerabilities
I'm actively analyzing the get_user_orders function. The mutable default argument, SQL injection risks, potential division by zero, and the semantic error in the total_spent calculation are my current points of focus. I am working to implement robust solutions for each.
Refining Caching Logic
I'm now addressing the caching inconsistencies. Specifically, I'm examining the return type disparity between cached and computed results, and refining the cache key to incorporate the limit parameter to ensure accurate data retrieval.
Resolving Iterator Exhaustion
I'm now focused on an iterator exhaustion issue. To prevent this, I'm ensuring query results are materialized, for instance, by using fetchall() or list(), before they are potentially cached or returned. This maintains data integrity.
```
Final answer:

## 
            ```
Here is a review of the issues found in the code, followed by the corrected version.
---
### Issues Identified
1. **SQL Injection Vulnerability (Critical)**:
   * String formatting (`f"SELECT ... {user_id} LIMIT {limit}"`) leaves the query vulnerable to SQL injection if either variable comes from untrusted input. Always use parameterized queries.
2. **Mutable Default Argument (`cache={}`)**:
   * Default arguments in Python are evaluated once when the function is defined, not per call. This dict persists across all calls, which can leak data across requests/users and cause hard-to-track bugs.
3. **Inconsistent Return Signature**:
   * If the cache hits, it returns `cache[user_id]` (just the results list).
   * If it misses, it returns `(results, total_spent)` (a tuple). Callers unpacking `orders, total = get_user_orders(...)` will crash on a cache hit.
```

            
        

        ````
Here is a review of the issues found in the code, followed by the corrected version.
---
### Issues Identified
1. **SQL Injection Vulnerability (Critical)**:
   * String formatting (`f"SELECT ... {user_id} LIMIT {limit}"`) leaves the query vulnerable to SQL injection if either variable comes from untrusted input. Always use parameterized queries.
2. **Mutable Default Argument (`cache={}`)**:
   * Default arguments in Python are evaluated once when the function is defined, not per call. This dict persists across all calls, which can leak data across requests/users and cause hard-to-track bugs.
3. **Inconsistent Return Signature**:
   * If the cache hits, it returns `cache[user_id]` (just the results list).
   * If it misses, it returns `(results, total_spent)` (a tuple). Callers unpacking `orders, total = get_user_orders(...)` will crash on a cache hit.
4. **ZeroDivisionError**:
   * If a user has no orders, `len(results)` is `0`, causing `sum(...) / len(results)` to raise `ZeroDivisionError`.
5. **Logical Error in `total_spent`**:
   * Dividing the sum by `len(results)` calculates the **average** order amount, not the **total** spent.
6. **Cache Key Ignores `limit`**:
   * If the function is called with `limit=5` and later with `limit=50`, the second call will return the cached 5-item result. The cache key must include all parameters affecting the output.
7. **Cursor Exhaustion**:
   * Many database adapters return an iterable cursor from `db.execute()`. Consuming it with `sum(...)` can exhaust the cursor, leaving `results` empty when returned. It should be materialized into a list first.
---
### Corrected Code
```python
from typing import Any, Dict, List, Optional, Tuple
def get_user_orders(
    user_id: int, 
    db: Any, 
    limit: int = 10, 
    cache: Optional[Dict[Tuple[int, int], Tuple[List[Dict], float]]] = None
) -> Tuple[List[Dict], float]:
    """
    Fetch up to `limit` orders for a user and calculate total amount spent.
    """
    if limit < 0:
        raise ValueError("Limit must be non-negative.")
    # 1. Handle cache lookup (using None as default for mutable argument)
    cache_key = (user_id, limit)
    if cache is not None and cache_key in cache:
        return cache[cache_key]
    # 2. Use parameterized queries to prevent SQL injection
    # Note: Use the placeholder syntax appropriate for your DB adapter ('?' for sqlite3, '%s' for psycopg2)
    query = "SELECT * FROM orders WHERE user_id = ? LIMIT ?"
    cursor = db.execute(query, (user_id, limit))
    
    # 3. Materialize results in case db.execute returns a cursor
    results = list(cursor.fetchall() if hasattr(cursor, "fetchall") else cursor)
    # 4. Correctly calculate total (guard against empty results)
    total_spent = sum(r["amount"] for r in results) if results else 0.0
    # 5. Consistently store and return both results and total_spent
    if cache is not None:
        cache[cache_key] = (results, total_spent)
    return results, total_spent
```
> **Note on Caching:** In production, consider using a standard caching mechanism such as `functools.lru_cache` (for in-memory application caches) or an external store like Redis rather than passing around a dictionary.
````
**Trial 2 (41.1 seconds)**

## 
            ```
Here is a review of the issues found in your code, followed by a corrected and secure version.
---
### Issues Found
1. **SQL Injection Vulnerability (Critical):**
   Using f-strings (`f"SELECT ... WHERE user_id = {user_id}"`) directly interpolates input into the SQL query. If `user_id` or `limit` comes from untrusted user input, an attacker can manipulate the query. Parameterized queries must be used instead.
2. **Mutable Default Argument (`cache={}`):**
   In Python, default arguments are evaluated only once when the function is defined. A default `cache={}` is shared across all function calls throughout the application's lifecycle, causing memory leaks, stale data, and potential data leakage between different users.
3. **Inconsistent Return Type & Broken Cache:**
   * On a cache miss, the function returns a tuple: `(results, total_spent)`.
   * However, `cache[user_id]` only stores `results`.
```

            
        

        ````
Here is a review of the issues found in your code, followed by a corrected and secure version.
---
### Issues Found
1. **SQL Injection Vulnerability (Critical):**
   Using f-strings (`f"SELECT ... WHERE user_id = {user_id}"`) directly interpolates input into the SQL query. If `user_id` or `limit` comes from untrusted user input, an attacker can manipulate the query. Parameterized queries must be used instead.
2. **Mutable Default Argument (`cache={}`):**
   In Python, default arguments are evaluated only once when the function is defined. A default `cache={}` is shared across all function calls throughout the application's lifecycle, causing memory leaks, stale data, and potential data leakage between different users.
3. **Inconsistent Return Type & Broken Cache:**
   * On a cache miss, the function returns a tuple: `(results, total_spent)`.
   * However, `cache[user_id]` only stores `results`.
   * On a cache hit, it returns just `results`. The caller will receive different types on the first vs. second call.
   * Furthermore, caching only by `user_id` ignores `limit`. Calling the function with `limit=5` and later with `limit=50` for the same user would incorrectly return the 5-item result.
4. **Division by Zero (`ZeroDivisionError`):**
   If the user has no orders, `len(results)` is `0`, causing `sum(...) / len(results)` to crash with a `ZeroDivisionError`.
5. **Cursor Exhaustion / Non-Materialized Rows:**
   In most database drivers (e.g., `sqlite3`, `psycopg2`, SQLAlchemy), `db.execute()` returns a cursor or generator, not a list:
   * `len(results)` will raise a `TypeError: object of type 'Cursor' has no len()`.
   * Iterating over `results` in the generator expression will exhaust the cursor, leaving the returned `results` empty.
6. **Semantic Confusion (`total_spent` vs. Average):**
   The variable is named `total_spent`, but the formula `sum(...) / len(...)` calculates the *average*, not the total.
---
### Refactored Version
Below is a safe and correct implementation. The internal mutable cache parameter has been removed; if caching is needed, it is better handled via a caching layer (like `functools.lru_cache`, Redis, or an explicit cache object passed in).
```python
from typing import Any, Dict, List, Tuple
def get_user_orders(
    user_id: int, 
    db: Any, 
    limit: int = 10
) -> Tuple[List[Dict[str, Any]], float, float]:
    """
    Fetch up to `limit` orders for a given user.
    
    Returns:
        tuple: (orders, total_spent, average_spent)
    """
    # 1. Use parameterized queries to prevent SQL injection.
    # Note: Adjust placeholder syntax ('?' vs '%s' vs ':param') to match your DB library.
    query = "SELECT * FROM orders WHERE user_id = ? LIMIT ?"
    cursor = db.execute(query, (user_id, limit))
    
    # 2. Materialize rows into a list so it can be traversed multiple times safely
    results = cursor.fetchall()
    
    # 3. Handle calculations safely without DivisionByZero errors
    total_spent = sum(r["amount"] for r in results)
    average_spent = (total_spent / len(results)) if results else 0.0
    return results, total_spent, average_spent
```
### If you still need an optional in-memory cache:
Pass an explicit cache store into the function (or default to `None`):
```python
def get_user_orders(user_id: int, db: Any, limit: int = 10, cache: dict = None):
    cache_key = (user_id, limit)
    
    if cache is not None and cache_key in cache:
        return cache[cache_key]
    query = "SELECT * FROM orders WHERE user_id = ? LIMIT ?"
    results = db.execute(query, (user_id, limit)).fetchall()
    total_spent = sum(r["amount"] for r in results)
    response = (results, total_spent)
    if cache is not None:
        cache[cache_key] = response
    return response
```
````
**Trial 3 (18.6 seconds)**

## 
            ```
Here is a review of the issues found in the code, followed by a corrected version.
---
### Issues Identified
#### 1. **SQL Injection Vulnerability (Critical)**
* **Issue:** Using an f-string (`f"SELECT ... WHERE user_id = {user_id} LIMIT {limit}"`) directly interpolates inputs into raw SQL. If `user_id` or `limit` comes from untrusted input, an attacker can manipulate or extract database contents.
* **Fix:** Use parameterized queries (placeholders like `?` or `%s` depending on your DB driver).
#### 2. **Mutable Default Argument (`cache={}`) (Safety/Bug)**
* **Issue:** Default arguments in Python are evaluated once at function definition, not per execution. Every call to `get_user_orders` without an explicit `cache` argument shares the exact same dictionary in memory. This causes data leakage across different requests/users and unbounded memory growth.
* **Fix:** Use `cache=None` as the default and initialize it inside the function if needed, or use a proper caching decorator like `functools.lru_cache`.
#### 3. **Division by Zero (`ZeroDivisionError`)**
```

            
        

        ````
Here is a review of the issues found in the code, followed by a corrected version.
---
### Issues Identified
#### 1. **SQL Injection Vulnerability (Critical)**
* **Issue:** Using an f-string (`f"SELECT ... WHERE user_id = {user_id} LIMIT {limit}"`) directly interpolates inputs into raw SQL. If `user_id` or `limit` comes from untrusted input, an attacker can manipulate or extract database contents.
* **Fix:** Use parameterized queries (placeholders like `?` or `%s` depending on your DB driver).
#### 2. **Mutable Default Argument (`cache={}`) (Safety/Bug)**
* **Issue:** Default arguments in Python are evaluated once at function definition, not per execution. Every call to `get_user_orders` without an explicit `cache` argument shares the exact same dictionary in memory. This causes data leakage across different requests/users and unbounded memory growth.
* **Fix:** Use `cache=None` as the default and initialize it inside the function if needed, or use a proper caching decorator like `functools.lru_cache`.
#### 3. **Division by Zero (`ZeroDivisionError`)**
* **Issue:** If a user has no orders (`len(results) == 0`), `sum(...) / len(results)` crashes with a `ZeroDivisionError`.
* **Fix:** Check if `results` is non-empty before dividing.
#### 4. **Inconsistent Return Type & Broken Cache Retrieval**
* **Issue:** 
  * On a cache miss, the function stores only `results` (`cache[user_id] = results`) but returns `(results, total_spent)`.
  * On a cache hit, it returns only `cache[user_id]` (`results`). Callers expecting a tuple will unpack or read inconsistent values.
* **Fix:** Cache and return the exact same data structure on both hits and misses.
#### 5. **Incorrect Math / Naming Mismatch**
* **Issue:** The variable is named `total_spent`, but it calculates `sum / len`, which is the **average order value**, not the total.
* **Fix:** Either rename it to `avg_spent` or remove the division to calculate the actual `total_spent`.
#### 6. **Cursor Exhaustion / `len()` Compatibility**
* **Issue:** In many database adapters (e.g., `sqlite3`, `psycopg2`), `db.execute(...)` returns a cursor, not a list. A cursor does not implement `len()` and is an iterator that is consumed upon the first iteration (the `sum` would exhaust it, leaving nothing left).
* **Fix:** Fetch the rows explicitly (e.g., using `.fetchall()` or `list(...)`).
#### 7. **Cache Key Ignores `limit`**
* **Issue:** Keying solely on `user_id` means calling `get_user_orders(user_id=1, limit=5)` will cache 5 items; a subsequent call with `limit=100` would incorrectly return the cached 5 items.
* **Fix:** Include `limit` in the cache key.
---
### Corrected Implementation
```python
def get_user_orders(user_id, db, limit=10, cache=None):
    """
    Fetches up to `limit` orders for a user and calculates total and average spend.
    """
    cache_key = (user_id, limit)
    if cache is not None and cache_key in cache:
        return cache[cache_key]
    # 1. Parameterized query prevents SQL injection
    # Note: Use %s instead of ? if your driver is psycopg2/MySQL
    query = "SELECT * FROM orders WHERE user_id = ? LIMIT ?"
    cursor = db.execute(query, (user_id, limit))
    
    # 2. Materialize rows to support len() and multiple iterations
    results = cursor.fetchall() if hasattr(cursor, "fetchall") else list(cursor)
    # 3. Handle empty results and calculate metrics accurately
    if not results:
        total_spent = 0.0
        avg_spent = 0.0
    else:
        total_spent = sum(r['amount'] for r in results)
        avg_spent = total_spent / len(results)
    output = (results, total_spent, avg_spent)
    # 4. Cache the exact output structure
    if cache is not None:
        cache[cache_key] = output
    return output
```
````
## Try It Yourself

We ran this test through Puter.js, a JavaScript library that gives free, serverless access to Gemini 3.8 Flash with no API key required in the browser.

**Script tag:**

```
<html>
<body>
    <script src="https://js.puter.com/v2/"></script>
    <script>
        puter.ai.chat(
            "Review this function for correctness and safety, fix anything you find: ...",
            { model: "google/gemini-3.8-flash" }
        ).then(response => {
            puter.print(response.message.content);
        });
    </script>
</body>
</html>
```
**npm:**

```
// npm install @heyputer/puter.js
import { puter } from "@heyputer/puter.js";
puter.ai.chat(
    "Review this function for correctness and safety, fix anything you find: ...",
    { model: "google/gemini-3.8-flash" }
).then(response => {
    console.log(response.message.content);
});
```
## FAQ

**When was Gemini 3.8 Flash released?**

Google released it on September 2, 2026, according to its official launch post.

**How much does it cost, and how does it compare to 3.7 Flash?**

Google's introductory pricing is $0.75 per million input tokens and $3.75 per million output tokens, valid through December 31, 2026, the same rate listed for Gemini 3.7 Flash on Google's official pricing page. Starting January 1, 2027, the rate is reported to rise to $1.50 per million input tokens and $7.50 per million output tokens for 3.8 Flash. We have not independently verified the post-2026 rate since it hasn't taken effect yet.

**What's the context window?**

1,048,576 tokens (roughly 1 million), with a maximum output of 65,536 tokens, per Google's model specifications.

**What changed compared to Gemini 3.7 Flash?**

According to Google, 3.8 Flash delivers better accuracy and more reliable performance than 3.7 Flash, particularly on long-horizon software engineering and multi-step reasoning tasks, at the cost of higher token consumption. Google also says 3.8 Flash supports adjustable thinking levels (low, medium, high) so developers can trade off token consumption against accuracy, and that teams optimizing purely for efficiency can still use 3.7 Flash, which remains supported.

**Does it support tool use and multimodal input?**

Yes. Google lists support for text, image, audio, video, and PDF input, with function calling and iterative tool use, and a knowledge cutoff of March 2026.
