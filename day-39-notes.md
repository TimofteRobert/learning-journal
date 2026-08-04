# Day 39 - Frontend Pagination (Part 2)

## What I learned

Today I improved the frontend pagination by making it safer and more user-friendly.

Instead of only changing pages, I started preventing invalid navigation and updating the pagination buttons depending on the current page.

## Previous button

I finished the logic for the Previous button.

The function now prevents the user from navigating before page 1.

```javascript
if (currentPage === 1) {
    return;
}
```

I learned that this is an example of **defensive programming**. Even if the button is somehow clicked, the function protects itself from performing an invalid action.

## Previous button state

I created a helper function:

```javascript
updatePreviousButton()
```

This enables or disables the Previous button depending on the current page.

When the current page is 1, the button becomes disabled.

## Next button state

I also created:

```javascript
updateNextButton()
```

Instead of checking how many notes are currently displayed, I learned that a better solution is to calculate the total number of pages.

```javascript
const totalPages = Math.ceil(noteStats.matching_notes / 10);
```

The Next button is disabled whenever the current page is equal to or greater than the last available page.

## Statistics reuse

I introduced a new variable:

```javascript
let noteStats = null;
```

The statistics retrieved from the backend are now stored and reused by other functions instead of requesting the same information again.

This allows pagination logic to use the number of matching notes without making additional API requests.

## Defensive programming

I also updated `nextPage()`.

Instead of relying only on the disabled button, the function now checks whether the current page is already the last page before increasing it.

This means both Previous and Next protect themselves from invalid navigation.

## Testing

I tested several pagination scenarios, including:

* One page of notes.
* Exactly ten notes.
* Multiple pages of notes.
* Searches with no matching notes.
* Searches with a single page of results.

While testing, I also discovered another edge case.

If the user is on page 3 and performs a search that only returns one page of results, the frontend still requests page 3.

## Next goals

* Reset the current page to page 1 whenever the search changes.
* Keep pagination synchronized with searching.
* Continue improving the pagination user experience.
