# Day 28 – Refactoring and Code Quality

## Objective

Learn what refactoring is, when duplicated code should (and should not) be removed, and how experienced developers evaluate code quality.

---

## What is refactoring?

Refactoring means improving the internal structure of code **without changing its behavior**.

The application should work exactly the same before and after the refactor.

Goals of refactoring:

- Improve readability
- Reduce duplicated code
- Make future maintenance easier
- Reduce the chance of bugs

---

## DRY Principle

**DRY** stands for:

> **Don't Repeat Yourself**

The idea is that repeated code usually indicates an opportunity to create reusable code.

However, DRY is a guideline, not an absolute rule.

---

## Rule of Three

A useful guideline for deciding when to refactor:

- First occurrence: write the code.
- Second occurrence: duplication may still be acceptable.
- Third (or more): consider refactoring.

Avoid creating abstractions too early.

---

## Not all duplication is equally important

When deciding what to refactor first, consider:

- How much code is duplicated.
- How many times it is duplicated.
- How likely it is to change.
- How difficult it is to maintain.

Priority example:

1. Large SQL query copied multiple times.
2. Small database connection pattern repeated.
3. Small dictionary duplicated.
4. Simple print statements.

---

## Connection handling in `notes_service.py`

Many service functions begin with:

```python
conn, cur = get_cursor()

try:
    ...
finally:
    close_cursor(conn, cur)
```

Although this is duplicated, it is:

- Short
- Easy to understand
- Consistent

For a small project like the Notes App, keeping this pattern is acceptable.

---

## Helper functions

Helper functions should return **useful information**, not implementation details.

Example:

Instead of returning:

- Database connection
- Cursor

A helper that executes a query should usually return:

- Query results

The caller should receive the data it needs rather than the tools used internally.

---

## Backend architecture review

Request flow:

```text
index.html
    ↓
script.js
    ↓
api.js
    ↓
notes.py (route)
    ↓
notes_service.py (service)
    ↓
Database
    ↑
notes_service.py
    ↑
notes.py
    ↑
api.js
    ↑
script.js
    ↓
HTML updates
```

Each layer has a single responsibility.

---

## Important lessons

- Refactoring changes structure, not behavior.
- Duplicate code is not automatically bad.
- Readability is often more important than removing every duplication.
- Large duplicated code should be prioritized over small duplicated code.
- Good abstractions simplify code instead of making it more complicated.
- Good developers balance DRY, readability, and maintainability.

---

## New concepts learned

- Refactoring
- DRY (Don't Repeat Yourself)
- Rule of Three
- Code duplication
- Maintainability
- Helper functions
- Responsibilities of each backend layer