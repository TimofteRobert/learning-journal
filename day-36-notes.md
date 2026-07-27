# Day 36 – Adding and Displaying Note Creation Dates

## Goal

Extend the Notes application so every note stores and displays the date it was created.

---

## Database

Instead of recreating the database, I updated the existing table using a migration.

I added a new column:

* `created_at`

The column uses PostgreSQL's `DEFAULT CURRENT_TIMESTAMP`, meaning the database automatically generates the creation date whenever a new note is inserted.

This is preferable to generating the date in the frontend because:

* the timestamp is permanently stored;
* every client receives the same value;
* the backend remains the source of truth.

---

## Backend

I updated the SQL queries so they return the new `created_at` value.

Modified functions:

* `get_notes()`
* `get_note_by_id()`
* `create_note()`
* `update_note()`

I also updated the helper function:

* `make_note()`

so every returned note now contains:

* `id`
* `title`
* `content`
* `created_at`

---

## Pydantic Models

I learned the difference between request models and response models.

`NoteCreate` describes the information sent **to** the backend.

`NoteResponse` describes the information returned **from** the backend.

Because the database generates both the `id` and the `created_at` timestamp, those fields belong only in the response model.

The client should never send them.

---

## Frontend

The frontend was updated to display the creation date for every note.

Inside `createNoteElement()` I added:

* ID
* Created at

under the note content.

The backend sends the timestamp in ISO format, while JavaScript converts it into a readable format before displaying it.

```javascript
const createdAt = new Date(note.created_at).toLocaleString();
```

I also learned that `toLocaleString()` automatically formats the date according to the user's operating system and browser settings.

For example, because my system uses English (US), my application displays:

```
Created at: 7/24/2026, 7:34:59 PM
```

Someone using a different locale may see the exact same timestamp in another format.

---

## async and await

Today I reviewed what `async` and `await` actually do.

### async

An `async` function allows asynchronous operations such as API requests.

JavaScript does not freeze the entire application while waiting for the server to respond.

### await

`await` pauses only the current async function until the requested operation finishes.

Without `await`, the function could continue executing before the required data is available.

A simple way to think about it is:

* `async` allows waiting without blocking the rest of the application.
* `await` tells JavaScript exactly where it must wait.

---

## Full Stack Data Flow

This feature passed through every layer of the application.

```
PostgreSQL
    ↓
notes_service.py
    ↓
notes.py
    ↓
api.js
    ↓
script.js
    ↓
index.html
    ↓
Browser
```

This reinforced how information travels from the database all the way to the user interface.

---

## What I Learned

* Use database migrations instead of recreating tables.
* Let PostgreSQL generate creation timestamps.
* Keep the database as the source of truth.
* Request models and response models have different responsibilities.
* The backend returns raw data.
* The frontend decides how data is displayed.
* JavaScript can format timestamps using `Date()` and `toLocaleString()`.
* One feature often requires changes across every layer of a full-stack application.
* I reviewed the purpose of `async` and `await` and now understand how they work together during API requests.
