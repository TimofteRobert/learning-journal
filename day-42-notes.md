# Day 42 - API Response Handling

## What I learned

Today I improved the organization of the frontend API layer.

The application already separates frontend behavior in `script.js` from backend communication in `api.js`.

The `script.js` file is responsible for frontend behavior such as UI updates, pagination, rendering, validation, searching, sorting, and handling user actions.

The `api.js` file is responsible for communicating with the backend API.

## API response handling

I noticed that several functions in `api.js` repeated the same response handling code.

The functions were checking whether the HTTP response was successful and then converting the response into JSON.

I created a helper function called `handleResponse()` to centralize this repeated behavior.

```javascript
async function handleResponse(response) {
    if (!response.ok) {
        throw new Error(`HTTP error: ${response.status}`);
    }

    return await response.json();
}
```

The helper checks the HTTP response and throws an error when the request was unsuccessful.

If the response is successful, it returns the JSON data from the backend.

## Using the helper

The helper is now used by:

* `getNotes()`
* `getNoteStats()`
* `createNoteApi()`
* `updateNoteApi()`

For example, instead of repeating the response handling inside `getNotes()`, it now calls `handleResponse()`.

This keeps the API functions focused on making their specific requests.

## DELETE request

`deleteNoteApi()` was kept separate from `handleResponse()`.

The DELETE endpoint currently does not return JSON, while `handleResponse()` expects a JSON response.

Because of this difference, forcing DELETE to use the helper would make the abstraction more complicated instead of making the code clearer.

This was a useful example of avoiding unnecessary abstraction.

## Separation between script.js and api.js

I also reinforced the separation between the two frontend files.

`script.js` handles frontend and application behavior.

`api.js` handles communication with the backend.

For example, `script.js` decides what should happen when the user deletes a note, while `api.js` performs the HTTP DELETE request.

This keeps the API layer independent from the user interface.

## Testing

I tested the application after the refactoring.

The following functionality continued to work:

* Loading notes.
* Searching.
* Sorting.
* Pagination.
* Creating notes.
* Updating notes.
* Deleting notes.

The behavior of the application remained the same.

## Avoiding overengineering

I also decided not to create another helper specifically for DELETE.

Although some code is still repeated, creating another abstraction for a single occurrence would not provide enough value for this project.

This reinforced the idea that removing every repeated line is not always a good reason to introduce another abstraction.

The goal is to keep the code readable and maintainable, not simply to make it as DRY as possible.

## Main takeaway

Today I practiced refactoring duplicated API response handling into a shared helper while keeping the API layer separate from frontend behavior.

I also practiced deciding when an abstraction is useful and when leaving a small amount of duplication is the better engineering choice.

## Next goals

* Continue building the application instead of over-polishing small details.
* Work on functionality that provides useful practical experience.
* Continue applying separation of responsibilities when it provides a real benefit.
