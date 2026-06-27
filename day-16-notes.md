# Day 16 - Client-side Search and Data Filtering

## Topics learned

Today I added a search feature to the Notes application.

The search is currently implemented on the frontend by filtering the notes already loaded from the backend.

## Client-side filtering

The application now:

* retrieves all notes from the API
* reads the current search text
* filters the notes in JavaScript
* displays only matching results

This approach is simple and suitable for small datasets.

## JavaScript array filtering

Learned how to use:

```javascript
filter()
```

The `filter()` method creates a new array containing only the elements that satisfy a condition.

The original array is not modified.

## String searching

Used:

```javascript
includes()
```

to determine whether the title or content contains the search text.

Also used:

```javascript
toLowerCase()
```

to make searching case-insensitive.

## Helper functions

Created helper functions to separate responsibilities.

Examples:

* `getSearchText()`
* `filterNotes()`

This keeps `loadNotes()` focused on loading data rather than processing it.

## User experience improvements

Added a message when no notes match the current search.

Instead of displaying an empty page, the application now informs the user that no matching notes were found.

## Separation of responsibilities

Current frontend responsibilities:

* UI helpers
* Validation
* Error handling
* Form helpers
* Search helpers
* Rendering
* CRUD operations
* API communication

Each section has a clear purpose, making the project easier to maintain.

## Current architecture

Frontend UI

↓

Search & Rendering

↓

API Layer

↓

FastAPI

↓

Service Layer

↓

PostgreSQL

## Next step

Move the search functionality to the backend using FastAPI query parameters so the server returns only matching notes instead of sending every note to the browser.
