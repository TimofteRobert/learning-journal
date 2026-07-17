# Day 31 - Coupling and Reusability

## What I learned

### Coupling

Coupling describes how much one piece of code depends on another.

A function with fewer dependencies is generally easier to reuse, test, and maintain.

Example of loose coupling:

```python
def get_note_title(note):
    return note["title"]
```

This function only depends on the `note` parameter.

Example of tighter coupling:

```python
def get_note_title():
    return current_user.current_project.selected_note["title"]
```

This depends on several external objects before it can work.

---

### Parameters vs hidden dependencies

Passing data as parameters is usually better than reading global objects or shared state.

A function with parameters may appear more complicated, but it is often more reusable because everything it needs is explicit.

---

### Reusability

A reusable function:

- receives what it needs as parameters
- performs one clear task
- has few dependencies
- can be copied into another project with minimal changes

---

### Dependencies are not always bad

A dependency is acceptable when it belongs to the same layer.

Examples:

- `script.js` depends on `api.js`
- `notes.py` depends on `notes_service.py`
- `notes_service.py` depends on the database

Bad examples:

- frontend executing SQL
- service updating HTML
- API layer manipulating database tables directly

The goal is not to eliminate dependencies, but to keep them in the correct direction.

---

### New question to ask

Besides asking:

- Does it work?
- Is it readable?
- Does it have one responsibility?

I should also ask:

**"What does this function depend on?"**

Thinking about dependencies helps identify coupling and reusability.

---

## Key takeaway

Good software is not just about making code work.

It is also about reducing unnecessary dependencies so functions and modules remain reusable, testable, and easier to maintain.