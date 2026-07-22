# Day 34 – Planning Feature Implementation

## Topics studied

Today I focused on how to safely introduce a new feature into an existing application.

The feature discussed was adding a `created_at` field to notes.

---

## Database changes

Instead of recreating the database, the preferred solution is to add a new column.

Advantages:

- existing data is preserved
- applications continue to work while the schema evolves
- backups remain a safety measure instead of part of the migration process

---

## Pydantic models

I reviewed the difference between request and response models.

`NoteCreate`

Contains only the information provided by the client.

`NoteResponse`

Contains everything returned by the backend, including generated fields such as:

- id
- created_at

The frontend should not decide these values.

---

## Implementation order

The implementation order I chose was:

1. Database
2. models.py
3. notes_service.py
4. notes.py
5. script.js

This order builds from the data layer outward and minimizes dependency problems.

---

## Compatibility

Adding a new database column is generally backward compatible because existing code only retrieves the fields it requests.

The frontend is the layer most likely to show problems if it expects a new field before the backend provides it.

---

## Main takeaway

Before writing code, it is useful to think about:

- where data should be created
- which layer owns that data
- how changes propagate through the architecture
- the safest implementation order