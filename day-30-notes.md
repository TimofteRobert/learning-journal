# Day 30 - Single Responsibility Principle (SRP)

## Main topic

Today focused on understanding the Single Responsibility Principle rather than writing new application code.

A function should have one clear responsibility. The number of lines or helper function calls is not what determines SRP. Instead, the important question is:

> What is the overall purpose of this function?

Examples discussed:

- `register_user()` has the responsibility of registering a user.
- Validation and saving are implementation steps toward that responsibility.
- `export_notes()` should actually perform an export (such as creating a PDF), not only retrieve notes.

## Key lessons

### Good function names

A well-named function should almost explain itself.

Example:

```python
def register_user():
```

The best description of this function is simply:

> Registers a user.

When that is possible, the function name is doing its job well.

### One responsibility vs multiple steps

A function may call several helper functions while still having one responsibility.

Example:

```python
def export_notes():
    notes = get_notes()
    create_pdf(notes)
```

Although there are multiple steps, they all contribute to one goal: exporting notes.

### Multiple responsibilities

Functions become harder to maintain when unrelated work is combined.

Example:

- register user
- send email
- write log
- backup database

These are different responsibilities and are better separated.

### Trade-offs

Not every design decision has a single correct answer.

Example:

```python
create_note()
update_statistics()
```

versus

```python
create_note()
```

where `create_note()` internally updates statistics.

Both approaches can be valid depending on the project's design goals. The important part is understanding and explaining the trade-offs.

## Personal observations

I noticed that I naturally started thinking about maintainability and consistency instead of only syntax.

When reviewing examples, I asked questions about:

- whether code is readable
- whether responsibilities are separated
- whether workflow order makes sense
- whether future changes would be easier

This is a different mindset from simply making code work.