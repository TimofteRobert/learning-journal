# Day 10 - Frontend CRUD with JavaScript

## Topics learned

Today I connected the browser frontend to the FastAPI backend and completed CRUD operations from the user interface.

Application flow:

Browser
↓
JavaScript
↓
FastAPI
↓
Service Layer
↓
PostgreSQL

## Fetch API

Learned how to use JavaScript fetch() to communicate with the backend.

Examples:

* GET /notes
* POST /notes
* PUT /notes/{id}
* DELETE /notes/{id}

The frontend sends HTTP requests and receives JSON responses.

## Async and Await

Learned:

* async functions
* await keyword

await pauses execution until the request finishes and the response is received.

## CORS

Encountered a browser security error when the frontend tried to communicate with FastAPI.

Learned that:

* browsers enforce Same Origin Policy
* FastAPI must explicitly allow frontend requests using CORSMiddleware

## DOM Manipulation

Used:

* document.getElementById()
* document.createElement()
* appendChild()
* innerHTML

to dynamically display notes in the page.

## Frontend State

Introduced:

editingNoteId

This variable stores which note is currently being edited.

Example:

editingNoteId = 5

means the user is editing note with id 5.

## CRUD Operations

Implemented:

### Create

POST request creates a new note.

### Read

GET request loads notes from PostgreSQL and displays them.

### Update

PUT request updates an existing note using editingNoteId.

### Delete

DELETE request removes a note and refreshes the page.

## JavaScript Template Literals

Learned to use:

`text ${variable}`

to insert variables into strings.

Example:

`/notes/${noteId}`

creates a URL using the current note id.

## Frontend Refactoring

Separated responsibilities:

loadNotes()

* fetches data

renderNotes()

* displays data

This follows the same separation of concerns principle used in the backend service layer.

## Important Result

The application now supports full CRUD functionality through the browser:

* Create
* Read
* Update
* Delete

without using FastAPI Swagger UI.
