# Day 44 — API Validation and Environment Configuration

## What I worked on

Today I continued improving the `hello-fastapi` Notes API by adding validation at the API boundary and validating the application's database configuration.

The main focus was making invalid requests fail early with appropriate HTTP responses instead of allowing invalid data to reach the service/database layer.

## Request validation

### Pydantic field validation

Updated `NoteCreate` in `app/models.py` to validate the minimum length of note fields:

* Title must contain at least 3 characters.
* Content must contain at least 5 characters.

Invalid create/update requests now return HTTP `422 Unprocessable Entity`.

I used Pydantic's `Field(min_length=...)` for this validation.

## HTTP status codes

The create endpoint now explicitly returns:

* `201 Created` when a note is successfully created.

The existing endpoints already handle missing notes with:

* `404 Not Found`

The tested behavior is:

* `GET /notes/{note_id}` with a nonexistent ID → `404`
* `PUT /notes/{note_id}` with a nonexistent ID → `404`
* `DELETE /notes/{note_id}` with a nonexistent ID → `404`

Invalid note data produces `422`.

## Query parameter validation

The `GET /notes` endpoint was updated to validate its query parameters.

### Page

The `page` parameter now requires an integer greater than or equal to 1.

Examples:

* `page=1` → valid
* `page=2` → valid
* `page=0` → `422`
* `page=-1` → `422`

I used FastAPI's `Query` with `ge=1`.

### Sort

The `sort` parameter is now restricted to the values actually supported by the service:

* `id`
* `id_desc`
* `title`
* `title_desc`

Invalid sort values now produce `422`.

I used Python's `Literal` type for this.

### Note ID

The `note_id` path parameter for GET, PUT, and DELETE now requires a positive integer.

Values such as `0` and negative numbers are rejected with `422`.

A nonexistent but valid positive ID continues to produce `404`.

## Environment variable validation

Updated `app/database.py` to verify that all required database environment variables exist when the application starts.

Required variables:

* `DB_HOST`
* `DB_PORT`
* `DB_NAME`
* `DB_USER`
* `DB_PASSWORD`

The application now raises a clear `RuntimeError` if one or more required variables are missing.

This makes configuration problems easier to identify instead of discovering them later when a database connection is attempted.

The actual values remain in `.env` and are not committed to Git.

## Testing

I tested the new validation through Swagger/OpenAPI.

Confirmed:

* Valid note creation → `201`
* Invalid note creation → `422`
* Valid note update → `200`
* Invalid note update → `422`
* Updating nonexistent note → `404`
* Deleting nonexistent note → `404`
* Invalid page values → `422`
* Invalid sort value → `422`
* Invalid note IDs → `422`
* Nonexistent valid note ID → `404`
* Application still connects to PostgreSQL correctly with the existing `.env`

## What I learned

* Pydantic can validate model fields using `Field`.
* FastAPI can validate query parameters with `Query`.
* `Literal` can restrict a parameter to a predefined set of values.
* FastAPI can validate path parameters using `Path`.
* HTTP status codes communicate the result of API operations.
* Environment variables can be validated when an application starts.
* Configuration should be kept outside the source code, especially secrets such as database passwords.

## Project structure

The backend currently uses:

```text
app/
├── main.py
├── database.py
├── models.py
├── routes/
│   └── notes.py
└── services/
    └── notes_service.py
```

The separation between routes, services, models, and database configuration is continuing to make the project easier to extend.

## Day 44 summary

The Notes API now performs more validation at the API boundary and provides clearer failures for invalid input and missing configuration.

The application functionality remains the same for valid requests, but invalid requests are handled more safely and predictably.
