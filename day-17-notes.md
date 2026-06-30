# Day 17 - Adding Search Functionality and Backend Filtering

## Topics learned

Today I added search functionality to the Notes application.

The goal was to allow searching notes by title or content while keeping responsibilities separated between the frontend, backend, and database.

## Backend search implementation

Added support for query parameters in FastAPI.

Example:


GET /notes?search=test


The route receives the search value:

```python
def get_all_notes(search: Optional[str] = None):

and passes it to the service layer.

Database filtering

The service layer now performs searching directly in PostgreSQL.

Used:

ILIKE

because it allows case-insensitive searching.

Example:

WHERE
    title ILIKE %s
    OR content ILIKE %s

The database now returns only notes that match the search text.

Frontend changes

Added a search input in index.html.

The input uses:

oninput="loadNotes()"

so every change in the search field triggers a new request.

The frontend now sends the search value to the backend instead of filtering all notes locally.

Before:

Database
    ↓
All notes
    ↓
JavaScript filtering
    ↓
Display results

After:

Database
    ↓
Search filtering
    ↓
Frontend displays results
Debugging lesson

During implementation, the backend was returning:

200 OK

but the frontend still showed:

Error: Could not load notes from server

The problem was not the backend.

The issue was in JavaScript:

A variable name mistake inside the filtering function caused a frontend error.

Learned that:

backend success does not always mean frontend success
browser console errors are important
frontend and backend need to be debugged separately
Code organization

Current structure:

Frontend:

api.js
handles communication with FastAPI
script.js
handles UI logic
rendering
user actions

Backend:

routes
handles HTTP requests
services
handles database operations
database
handles PostgreSQL connection
Comments and learning code

Kept some previous commented code intentionally.

During learning, old approaches can be useful because they show:

what was tried before
what changed
why a new approach was chosen

In a production project old code would usually be removed because Git keeps the history, but during learning it can help track progress.

Current project status

The Notes application now has:

CRUD operations
PostgreSQL database
FastAPI backend
frontend interface
separated API layer
validation
error handling
search functionality
Next step

Continue improving the application architecture and learn more professional development practices.