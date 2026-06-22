# Day 12 - Frontend Architecture and Separation of Concerns

## Topics learned

Today I improved the frontend architecture of the Notes application.

The goal was not to add new features but to organize the code better and reduce coupling between different responsibilities.

## Separation of concerns

A file should have a clear responsibility.

Before:

script.js handled:

* API communication
* UI rendering
* form management
* user actions

After:

api.js handles:

* communication with FastAPI

script.js handles:

* UI behavior
* rendering notes
* form interactions

## API layer

Created:

```javascript
const API_URL = "http://127.0.0.1:8000";
```

and moved API calls into dedicated functions:

* getNotes()
* createNoteApi()
* updateNoteApi()
* deleteNoteApi()

Benefits:

* centralized backend communication
* easier maintenance
* easier future changes

## UI helper functions

Created helper functions:

### setStatus()

Updates the status message shown to the user.

### getFormData()

Reads form values and returns a note object.

### clearForm()

Resets the form after create/update operations.

Benefits:

* less duplicated code
* easier maintenance

## Rendering organization

Created:

```javascript
createNoteElement(note)
```

This function is responsible for creating a single note card.

renderNotes() is now responsible only for rendering the list.

Benefits:

* cleaner code
* single responsibility
* easier future changes

## Comments

Learned that useful comments explain:

* intent
* purpose
* non-obvious decisions

Comments should not explain obvious code.

## Frontend architecture

Current structure:

Frontend

UI
↓
API layer
↓
FastAPI
↓
Service layer
↓
Database

The frontend is beginning to follow the same architectural principles used in the backend.

## Current project status

Frontend:

* HTML
* CSS
* JavaScript
* Fetch API
* CRUD interface
* UI helpers
* API layer separation

Backend:

* FastAPI
* Service layer
* PostgreSQL integration

Infrastructure:

* Docker
* Environment variables

## Next step

Continue improving frontend architecture and prepare for more advanced frontend development concepts.
