# Day 22

## What I worked on

Today I focused on refactoring the backend code to reduce duplication and make it easier to maintain.

### Changes made

* Created a `close_cursor(conn, cur)` helper function.
* Replaced repeated `cur.close()` and `conn.close()` calls with the new helper.
* Kept the application's behavior the same while making the code cleaner.
* Discussed why not every repeated line should become a helper function.

## What I learned

* Helper functions are useful when the same logic appears in multiple places.
* The DRY (Don't Repeat Yourself) principle helps reduce duplicated code.
* Abstraction hides implementation details behind simple functions like `get_cursor()`, `make_note()`, and `close_cursor()`.
* A helper function should make the code easier to understand, not just shorter.
* Some repeated code is acceptable if creating a helper would make the code more complicated than the original.

## Concepts reviewed

* DRY (Don't Repeat Yourself)
* Abstraction
* Helper functions
* Database connections and cursors
* `conn.commit()`
* `try` / `finally` for resource cleanup

## Mini challenges

* Identified that six functions repeated the same connection cleanup code.
* Explained why `close_cursor(conn, cur)` needs two arguments.
* Understood that changing one helper function updates the behavior everywhere it is used.
* Explained why creating a helper for `conn.commit()` would not improve the code.
* Learned about Python docstrings and how IDEs display them when hovering over functions.

## Reflection

Today's work was mostly about improving code quality rather than adding new features. I learned that good programming is not only about making code work, but also about making it easier to read, maintain, and modify in the future. I also became more comfortable recognizing when a helper function is useful and when it would only add unnecessary complexity.
