# Day 40 - Search and Pagination Integration

## What I learned

Today I connected searching with pagination and continued improving the frontend architecture by reducing duplicated code.

Instead of treating search and pagination as separate features, I learned that they must work together.

## Resetting pagination

I created a helper function:

```javascript
function resetPagination() {
    currentPage = 1;
    updatePageNumber();
    updatePreviousButton();
}
```

Whenever the search or sorting changes, the application now returns to page 1.

This prevents situations where the user is viewing page 3, performs a search that only has one page of results, and the frontend still requests page 3.

## Search and sorting

I created two functions:

```javascript
searchNotes()
sortNotes()
```

Both currently perform the same actions:

* Reset pagination.
* Refresh the page.

Although their implementation is identical, I learned they represent different user actions.

Keeping them as separate functions gives each one a single responsibility and allows them to evolve independently later without affecting the other.

## Refactoring duplicated code

I also identified another opportunity for refactoring.

Both `searchNotes()` and `sortNotes()` execute the same sequence of operations.

A helper function such as:

```javascript
refreshAfterFilterChange()
```

can later contain the shared logic.

This follows the DRY (Don't Repeat Yourself) principle while keeping the public functions meaningful.

## Another possible helper

I also noticed that both `nextPage()` and `previousPage()` update the pagination interface in the same way.

This could later be extracted into another helper function responsible only for updating the pagination UI.

## Backend requests

While discussing the search feature, I realized that using `oninput` causes a backend request after every key press.

For example, typing a ten-letter word currently generates around twenty backend requests because both notes and statistics are requested each time.

I learned that this can later be improved using **debouncing**, allowing the frontend to wait briefly until the user stops typing before sending requests.

## Programming concepts

Today reinforced several important software engineering ideas:

* Single Responsibility Principle.
* High cohesion.
* Low coupling.
* DRY (Don't Repeat Yourself).
* Refactoring duplicated code into helper functions.
* Thinking about application behavior instead of only making code work.

## Next goals

* Extract the shared search/sort logic into a helper function.
* Create a helper for repeated pagination UI updates.
* Implement search debouncing to reduce unnecessary backend requests.
