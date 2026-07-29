# Day 37 – Backend Pagination

## What I learned

Today I implemented the backend part of pagination.

### SQL Pagination

I learned that PostgreSQL uses:

* LIMIT → maximum number of rows returned
* OFFSET → number of rows skipped before returning rows

Example:

```sql
SELECT *
FROM notes
LIMIT 10
OFFSET 20;
```

This returns the third page when each page contains 10 notes.

---

## Formula

To calculate the offset:

```text
offset = (page - 1) * limit
```

Examples:

* Page 1 → Offset 0
* Page 2 → Offset 10
* Page 3 → Offset 20

---

## Updating the Service Layer

I modified `notes_service.py`.

`get_notes()` now accepts:

* search
* sort
* limit
* offset

Both SQL queries now use:

```sql
LIMIT %s
OFFSET %s
```

The values are safely passed as SQL parameters.

I also learned that only search strings use:

```python
f"%{search}%"
```

because they are used with `ILIKE`.

Numeric values such as `limit` and `offset` are passed directly.

---

## Refactoring the Route

Initially the route exposed:

* limit
* offset

After discussing the design, I refactored the route so the frontend only sends:

```text
page
```

The route now calculates:

```python
limit = 10
offset = (page - 1) * limit
```

before calling the service.

This keeps each layer responsible for its own task.

---

## Layer Responsibilities

Frontend

* Works with page numbers.

Route

* Converts page into limit and offset.

Service

* Uses limit and offset because those match SQL.

Database

* Executes LIMIT and OFFSET.

---

## API Design

After the refactoring, Swagger now exposes:

* search
* sort
* page

instead of exposing database-specific concepts like limit and offset.

The API is easier to understand while the service layer remains unchanged.

---

## Validation Discussion

We discussed how to handle invalid page numbers.

Example:

```
page = 0
page = -5
```

Instead of silently fixing invalid values, a REST API should reject invalid input and return an error.

FastAPI can perform this validation automatically before the route function executes.

---

## Key Takeaways

* Separate API design from database implementation.
* Translate user-friendly concepts into database concepts inside the route layer.
* Keep the service focused on communicating with the database.
* LIMIT and OFFSET belong to SQL.
* Page numbers belong to the user interface.
