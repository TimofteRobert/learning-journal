# Day 21

## What I built

* Added a `/notes/stats` endpoint that returns the total number of notes.
* Displayed the total number of notes in the frontend.
* Created a `loadStats()` function to request statistics from the backend.
* Updated the statistics automatically after creating, updating and deleting notes.
* Refactored repeated refresh logic into a reusable `refreshPage()` helper function.

## What I learned

* `COUNT(*)` counts rows in a SQL table.
* FastAPI matches routes from top to bottom, so more specific routes should be defined before dynamic routes.
* The frontend does not automatically know when the database changes; it must request updated data from the backend.
* Helper functions reduce duplicated code and make future changes easier.
* Separating responsibilities (fetching data vs displaying data) makes code easier to maintain and reuse.

## New concepts

* SQL `COUNT(*)`
* Backend state vs frontend state
* Route matching order
* DRY (Don't Repeat Yourself)
* Separation of concerns
* UI refresh after API calls

## Mini challenges

* Understood why deleting a note requires refreshing the UI.
* Identified a variable name typo (`reponse` vs `response`).
* Explained why helper functions simplify future changes.
* Explained why separating data fetching from rendering improves code organization.
