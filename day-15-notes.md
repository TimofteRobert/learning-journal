# Day 15 - Cleaner Frontend Architecture

## Topics learned

Today I focused on improving the frontend architecture instead of adding new features.

The goal was to make the JavaScript code cleaner, easier to maintain, and closer to real-world development practices.

## Event handling

Previously, note buttons used inline JavaScript:

```html
<button onclick="...">
```

Now the buttons are created in JavaScript using:

```javascript
addEventListener()
```

This separates HTML structure from JavaScript behavior.

Benefits:

* cleaner HTML
* better separation of concerns
* easier maintenance
* follows modern frontend practices

## DOM manipulation

Continued working with:

* document.createElement()
* appendChild()
* addEventListener()

Instead of embedding all behavior directly inside HTML.

## UI improvements

Improved status messages by introducing CSS classes.

Created helper functions:

* setStatus()
* setError()

These functions now update both the message and its visual style.

## DRY principle

Learned the DRY (Don't Repeat Yourself) principle.

Instead of repeating:

* reading form data
* validating form data

Created a helper function:

```javascript
getValidatedNote()
```

This reduces duplicated code and makes future changes easier.

## Code organization

The frontend is now organized into sections:

* State
* UI helpers
* Error handling
* Validation
* Form helpers
* Rendering
* CRUD operations

This makes the file easier to navigate and maintain.

## Current architecture

Frontend

HTML
↓

JavaScript UI

↓

API layer

↓

FastAPI

↓

Service layer

↓

PostgreSQL

## What I learned

Adding new features is only one part of software development.

Refactoring existing code improves readability, maintainability, and prepares the project for future growth without changing its functionality.
