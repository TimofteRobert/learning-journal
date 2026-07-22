# Day 33 – Backend Design Decisions

## Topics studied

Today I focused on planning a new feature before writing code.

The feature is adding a `created_at` timestamp to every note.

---

## Request flow review

I reviewed the request flow of the Notes application from memory.

General flow:

index.html
→ script.js
→ api.js
→ notes.py
→ notes_service.py
→ database
→ notes_service.py
→ notes.py
→ api.js
→ script.js
→ DOM updated

---

## Backend ownership

I learned that some information should be owned by the backend instead of the frontend.

For a creation timestamp, the database or backend should generate it instead of the browser.

Reasons:

- every client receives consistent data
- users cannot manipulate the timestamp
- different browsers or system clocks do not affect stored data

---

## Layer responsibilities

For adding `created_at`:

- Database stores the value.
- Models describe the new field.
- Service reads and writes the value.
- Routes expose the value.
- Frontend displays the value.

Each layer has its own responsibility.

---

## Main takeaway

The backend should be the source of truth for data that represents the system itself, such as IDs and creation timestamps.
