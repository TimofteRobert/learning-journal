# Day 38 - Frontend Pagination (Part 1)

## What I learned

Today I connected the frontend to the backend pagination.

The backend already accepted a page number, converted it into `limit` and `offset`, and retrieved the correct notes from the database. Today I started making the frontend use that functionality.

## Pagination state

I created a new variable:

```javascript
let currentPage = 1;
```

The frontend now keeps track of the current page independently from the backend.

## Next and Previous buttons

I created two functions:

* `nextPage()`
* `previousPage()`

Their responsibility is to change the current page and reload the notes.

For now:

* Next increases the page number.
* Previous decreases the page number.

Later I will prevent the page from going below page 1.

## Page number helper

I created a helper function:

```javascript
updatePageNumber()
```

This updates the page number displayed in the HTML.

I learned that helper functions should be grouped together with other UI helper functions because they improve code organization.

## Event listeners

Instead of using `onclick` attributes, I connected the pagination buttons with JavaScript using:

```javascript
addEventListener()
```

The event listeners are attached once when the application starts.

## Refresh flow

The current flow is:

1. User clicks Next.
2. `currentPage` increases.
3. The page number is updated.
4. `loadNotes()` is called.
5. The backend receives the requested page.
6. SQL returns the correct notes.
7. The frontend renders the new notes.

## Code organization

Today I also understood that comments should explain the purpose of code instead of describing obvious instructions.

Good comments describe sections like:

* UI helper functions
* Page navigation
* Note operations

instead of commenting every individual line.

## Next goals

* Prevent navigating below page 1.
* Disable Previous when page 1 is selected.
* Handle empty pages when there are no more notes.
* Connect pagination correctly with search and sorting.
