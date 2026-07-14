# Day 29 - Layers, Refactoring, and Data Flow

## Topics Covered

- Reviewed the complete request/response flow:
  - index.html
  - script.js
  - api.js
  - FastAPI routes
  - service layer
  - PostgreSQL
  - response back to the frontend

- Discussed why changing databases (e.g. PostgreSQL to SQLite) mainly affects the service layer.

- Learned why helper functions should represent meaningful concepts rather than simply reduce repeated lines.

- Compared good helper functions (`make_note()`) with poor helper candidates (`rollback()`).

- Identified repeated transaction patterns using:
  - `get_cursor()`
  - `try`
  - `except`
  - `finally`
  - `close_cursor()`

- Learned that some repeated code should remain unchanged until a better abstraction exists.

- Discussed why `make_note()` centralizes the construction of note dictionaries.

- Understood that adding a new returned field (such as `created_at` or `author`) is much easier when object creation is centralized.

- Learned that API changes usually begin where the data originates: the SQL query.

- Reviewed which application layers require modification when adding new data fields.

## Key Takeaways

- Refactoring is about improving maintainability, not simply reducing repeated lines.

- A good helper function should:
  - hide complexity,
  - represent a meaningful concept,
  - reduce duplicated logic,
  - centralize future changes.

- The service layer is responsible for database-specific code.

- The frontend should only display data; it should not know how the database works.

- When adding new information to an API response, start from the database and follow the data through each layer until it reaches the UI.

## Progress

I am becoming more comfortable identifying:

- repeated patterns,
- good vs. unnecessary refactoring,
- how data moves through every layer of the application,
- where new features should be implemented.

Today's focus was understanding software design decisions rather than writing new features.