# Day 41 - Frontend Async Operations

## What I learned

Today I learned about handling multiple independent asynchronous operations in the frontend.

The main example was the `refreshPage()` function, which loads both the notes and their statistics.

## Sequential asynchronous operations

Originally, `refreshPage()` waited for `loadNotes()` to finish before starting `loadStats()`.

The flow was:

1. Start `loadNotes()`.
2. Wait for it to finish.
3. Start `loadStats()`.
4. Wait for it to finish.

The two operations were independent because `loadStats()` does not need the result of `loadNotes()`, and `loadNotes()` does not need the result of `loadStats()`.

## Promise.all()

I changed `refreshPage()` to use `Promise.all()`.

```javascript
async function refreshPage() {
    await Promise.all([
        loadNotes(),
        loadStats()
    ]);
}
```

`Promise.all()` allows multiple asynchronous operations to start at the same time.

The frontend then waits until all of them have finished before continuing.

Instead of:

```text
loadNotes -> wait -> loadStats -> wait
```

the operations can now run like:

```text
loadNotes  -------->
loadStats  ------>
             |
             v
        both finished
```

This can reduce unnecessary waiting when multiple operations are independent.

## Why this matters

For this small application, the performance difference is not important.

However, the concept is useful in larger applications where multiple independent asynchronous operations may take noticeable amounts of time.

For example, if one request takes 500 ms and another takes 300 ms, running them sequentially can take approximately 800 ms, while running them concurrently can take approximately 500 ms.

The important lesson is not to optimize everything automatically, but to understand when concurrent execution is appropriate.

## Avoiding overengineering

Today I also made an architectural decision not to redesign the API just to eliminate the two requests made during a search.

A search currently needs:

* The notes that should be displayed.
* The statistics needed to calculate the number of pages.

These are retrieved through two requests.

For this project, keeping the two requests is reasonable because the application is small and the existing design is clear and functional.

Spending significant time redesigning this only for a small performance improvement would have a lower value than continuing to build the project and gaining experience from new projects.

This reinforced an important engineering principle:

> Good engineering is not about optimizing everything. It is about making improvements that are justified by the requirements and the context.

## Testing

I tested the application after changing `refreshPage()` to use `Promise.all()`.

The application continued to work correctly.

## Main takeaway

Today I learned that independent asynchronous operations can often be executed concurrently with `Promise.all()` instead of waiting for each operation sequentially.

I also learned that knowing when **not** to optimize is an important part of software engineering.

## Next goals

* Continue improving the application where improvements provide useful learning.
* Avoid spending excessive time on extremely unlikely edge cases or unnecessary optimization.
* Continue building the skills needed for larger projects.
