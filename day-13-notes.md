# Day 13 - Error Handling and User Feedback

## Topics learned

Today I improved the reliability of the frontend application by adding error handling and user feedback.

The goal was to make the application behave correctly when something goes wrong instead of assuming every operation succeeds.

## Error handling in JavaScript

Learned how to use:

```javascript
try {
    // operation
}
catch (error) {
    // handle error
}
```

This allows the application to react gracefully when an API request fails.

Used try/catch in:

* createNote()
* updateNote()
* deleteNote()
* loadNotes()

## Error helper function

Created:

```javascript
function setError(message)
```

This centralizes error messages displayed to the user.

Benefits:

* less duplicated code
* consistent error display
* easier maintenance

## API layer validation

Learned that fetch() does not automatically fail for HTTP errors such as:

* 400 Bad Request
* 404 Not Found
* 500 Internal Server Error

Added:

```javascript
if (!response.ok) {
    throw new Error(...)
}
```

to API functions.

This allows frontend code to detect backend failures correctly.

## User feedback

Added status messages to communicate application state.

Examples:

* Loading notes...
* Creating note...
* Updating note...
* Deleting note...
* Ready
* Error messages

Benefits:

* better user experience
* users know what the application is doing
* reduces confusion during slow operations

## Loading state concept

Learned that users should receive feedback while waiting for operations to complete.

Without feedback:

User clicks button
↓
Nothing happens visually

With feedback:

User clicks button
↓
Status message appears
↓
Operation completes

## Separation of concerns

Continued improving frontend architecture.

Responsibilities:

api.js

* backend communication
* HTTP validation
* request handling

script.js

* UI updates
* rendering
* state management
* user interactions

## Comments

Continued using meaningful comments.

Comments should explain:

* intent
* purpose
* important logic

Comments should not explain obvious syntax.

## Important concept

Professional applications handle both:

* success cases
* failure cases

A good developer thinks not only about:

"What happens when everything works?"

but also:

"What happens when something fails?"

## Current project status

Frontend:

* HTML
* CSS
* JavaScript
* Fetch API
* CRUD interface
* API layer
* Error handling
* Loading states
* User feedback

Backend:

* FastAPI
* Service layer
* PostgreSQL integration

Infrastructure:

* Docker
* Environment variables

## Next step

Implement frontend validation and continue learning modern JavaScript concepts that prepare for React.
