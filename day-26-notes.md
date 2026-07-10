# Day 26

## What I learned

### Helper functions and code organization

* Reviewed why helper functions make code easier to read and maintain.
* Learned that helper functions should remove repeated logic while keeping responsibilities small and focused.
* Discussed how to organize helper functions as a project grows.

### Understanding application flow

* Reviewed how a request travels through the application:

  * Frontend
  * Route
  * Service
  * Database
  * Response back to the frontend
* Practiced identifying which layer is responsible for each part of the process.

### Extending an existing feature

* Started extending the statistics endpoint to support search.
* Modified the route so it accepts an optional `search` parameter.
* Updated the service to count notes matching the search text.
* Learned that when no search is provided, the matching count should equal the total number of notes instead of returning `None`.

### Frontend observations

* Learned that JavaScript template literals (`` `${...}` ``) replace placeholders with actual values.
* Learned that `textContent` displays plain text and does not render HTML such as `<br>`.
* Continued following how data moves from the frontend to the backend and back again.

## Key ideas

* Every layer has a specific responsibility.
* Data only moves through the application when each function explicitly passes it to the next one.
* Designing the application's behavior is just as important as writing the code itself.

## Next step

Complete the search statistics feature by connecting the frontend so it sends the current search text when requesting statistics. This will allow the "Matching notes" counter to update automatically while typing.
