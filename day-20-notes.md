# Day 20 - Refactoring with Helper Functions

## What I learned

Today I focused on improving the backend code without changing how the application behaves.

### Helper functions

I reused the `make_note()` helper in multiple CRUD functions instead of creating note dictionaries manually each time.

Functions updated:
- get_notes()
- get_note_by_id()
- create_note()
- update_note()

This reduced duplicated code and made the service layer cleaner.

### get_cursor()

I also reused the `get_cursor()` helper so every function creates database connections in the same way.

### DRY Principle

DRY stands for "Don't Repeat Yourself".

Instead of repeating the same dictionary creation in multiple places, I now have one helper function responsible for creating note dictionaries.

If I need to change the structure of a note later, I only need to update one function instead of several.

### Single Responsibility Principle

Each helper has one clear responsibility.

- `get_cursor()` creates database connections.
- `make_note()` creates note dictionaries.
- CRUD functions perform database operations.

This makes the code easier to read and maintain.

### Dictionaries are mutable

I learned that dictionaries can be modified after they are created.

Example:

```python
person = {
    "name": "Alice"
}

person["age"] = 25
```

Results in:

```python
{
    "name": "Alice",
    "age": 25
}
```

This is why `update_note()` can create a note with `make_note()` and then add a `"message"` field afterward.

## Reflection

Today's work didn't add new features, but it made the code much cleaner.

I now better understand why experienced developers use helper functions and refactor duplicated code. Changing one helper is much easier than updating the same code in multiple places.