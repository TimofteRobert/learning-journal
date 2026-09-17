# Day 50 – API Error Handling and Initial Tests

## What I worked on

* Improved the frontend `handleResponse()` helper to handle empty or non-JSON responses safely.
* Updated `deleteNoteApi()` to use the shared `handleResponse()` helper.
* Updated the frontend UPDATE and DELETE functions to display useful error messages from the API.
* Ensured the notes page refreshes after a successful deletion, not after a failed one.
* Installed pytest and added it to `requirements.txt`.
* Created a `tests/` directory and added initial API tests.

## Tests

* `GET /notes` returns HTTP 200.
* An UPDATE request with invalid data returns HTTP 422.

**Result:** 2 tests passed.

## What I learned

* Centralised API response handling makes error handling more consistent.
* FastAPI validates request data before executing the route.
* FastAPI's `TestClient` can be used to test API endpoints without manually opening the frontend.
* Tests that access a real database need care so they do not depend on or modify existing project data.

## Next steps

* Review the remaining core project requirements.
* Prepare the README, screenshots, and demo.
* Move on to the next portfolio project once this one is demonstrable.
