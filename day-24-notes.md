# Day 24 Notes

## What I learned

- Reviewed how `None` propagates through function calls.
- Learned why forgetting to check for `None` can cause runtime errors.
- Introduced Python exception handling (`try` / `except`).
- Learned that exceptions stop execution immediately and transfer control to an `except` block.
- Understood the difference between returning `None` and raising an exception.
- Discussed why helper functions improve readability and reduce duplicated code.
- Reviewed the architecture of the Notes application from frontend to database.

## Mini challenges

- Traced how `None` moves through multiple function calls.
- Predicted the output of several `try` / `except` examples.
- Explained why accessing `note["title"]` fails when `note` is `None`.
- Reviewed helper functions and why they follow the DRY principle.

## Takeaways

- Not every error should silently return `None`; sometimes raising an exception is the better design.
- Helper functions are useful because they centralize repeated logic.
- Understanding program flow is becoming easier before actually running the code.

## Next goals

- Learn Python exceptions in more depth.
- Practice using `raise`, `try`, and `except`.
- Continue improving backend architecture and code organization.