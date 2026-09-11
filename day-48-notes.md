# Day 48 - Frontend User Integration and Timezone-Aware Timestamps

## What I worked on

Today I connected the frontend to the user functionality added to the Notes API and completed the integration between notes and users.

### 1. User selector in the frontend

The note form now contains an owner selector.

The frontend loads users from:

```text
GET /users
```

The returned users are used to populate the `<select>` element dynamically.

Instead of hardcoding users in the HTML, the frontend now gets the current users from the backend.

This makes the frontend depend on the API rather than duplicated user data.

### 2. Note ownership

Notes now contain a `user_id` identifying their owner.

When creating a note, the frontend sends:

```javascript
{
    title,
    content,
    user_id
}
```

The selected owner is also displayed when rendering notes.

A helper function looks up the username from the loaded users:

```javascript
getUsername(userId)
```

This keeps the API response simple because the note contains the user's ID, while the frontend can display the corresponding username.

### 3. Editing existing notes

When editing a note, the frontend now fills the owner selector with the note's existing `user_id`.

This means the form reflects the current state of the note rather than resetting the owner selection.

The backend update operation currently keeps the existing owner rather than changing ownership during a normal note update.

### 4. Timezone-aware timestamps

The `created_at` column was changed to a timezone-aware PostgreSQL timestamp.

The migration used:

```python
sa.DateTime(timezone=True)
```

and converted the existing values using:

```sql
created_at AT TIME ZONE 'UTC'
```

The frontend converts the timestamp returned by the API into the user's local time:

```javascript
new Date(note.created_at).toLocaleString()
```

This separates storage from presentation:

* PostgreSQL stores the timestamp consistently.
* The API returns the timestamp.
* The browser displays it in the user's local timezone.

### 5. API error handling

The frontend API helper was improved so JSON error responses from FastAPI can be handled consistently.

`handleResponse()` now checks:

```javascript
if (!response.ok)
```

and uses the API's `detail` message when available.

This allows frontend operations such as note creation to display the actual backend validation error instead of always showing a generic error.

### 6. Frontend state and refresh behaviour

The frontend now refreshes notes and statistics after create, update, delete, search, and sorting operations.

Pagination is also reset when the search or sorting criteria change.

One important debugging lesson from today was that changing frontend JavaScript or HTML files does not depend on Uvicorn's Python `--reload`.

The browser needs to reload the frontend files separately.

## What I learned

* How to populate a frontend `<select>` dynamically from an API.
* How a foreign-key relationship can be represented in a frontend application using an ID.
* How to display related data by mapping an ID to an object loaded from another endpoint.
* Why timestamps should be stored with timezone information when an application may be used across different timezones.
* The difference between backend hot reload and browser/frontend reload.
* How frontend error handling can expose useful validation messages returned by a REST API.
* How frontend state such as the selected owner and current page needs to be preserved or deliberately reset when the UI is refreshed.

## Architecture

The current note flow is approximately:

```text
Frontend
   │
   ├── GET /users
   │       └── Load available users
   │
   ├── POST /notes
   │       └── Create note with user_id
   │
   ├── GET /notes
   │       └── Load notes with user_id
   │
   └── PUT /notes/{id}
           └── Update note data

FastAPI
   │
   ├── Routes
   │
   ├── Services
   │
   └── PostgreSQL
           ├── users
           └── notes
                    └── user_id → users.id
```

## Key takeaway

The Notes App is becoming a proper multi-table application rather than a simple CRUD example.

The frontend, API, service layer, and database now work together around a real relationship between users and notes, while timestamps are handled consistently between the database, API, and browser.
