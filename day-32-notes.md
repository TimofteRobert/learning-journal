# Day 32 – Combining SRP, Coupling and Cohesion

## What I learned

Today I combined the three software design principles I have been studying:

- Single Responsibility Principle (SRP)
- Coupling
- Cohesion

Instead of evaluating each one separately, I practiced analyzing code using all three together.

---

## Single Responsibility Principle (SRP)

A function should have one clear responsibility.

Creating a note may internally validate and save it because both actions are part of the same responsibility: creating a valid note.

Adding unrelated actions such as sending emails, writing logs or updating statistics inside the same function usually violates SRP.

---

## Coupling

A function should depend on as few parts of the application as possible.

Example:

```python
def print_note_title(note):
    print(note["title"])
```

is more loosely coupled than

```python
def print_selected_note_title():
    note = get_selected_note()
    print(note["title"])
```

because the second function depends on another function before it can perform its work.

---

## Cohesion

Functions inside the same file should naturally belong together.

Example of high cohesion:

- create_note()
- update_note()
- delete_note()
- get_notes()

All of them belong inside `notes_service.py` because they manage notes.

A file containing note management, newsletter sending, image resizing and invoice generation would have low cohesion because the functions belong to different responsibilities.

---

## Applying all three principles

When reviewing code, I should ask:

1. Does this function have one responsibility? (SRP)
2. Does it depend on as little as possible? (Coupling)
3. Does it belong in this file? (Cohesion)

Good software design satisfies all three.

---

## Design discussion

I analyzed where a function like

```python
send_note_to_friend(note, email)
```

should belong.

Possible locations:

- notes_service.py
- email_service.py
- sharing_service.py

I concluded that if sharing by email is the only sharing feature, placing it in `email_service.py` is reasonable because its primary responsibility is sending emails.

If the application later supports multiple sharing methods (email, links, Slack, Teams, etc.), creating a dedicated `sharing_service.py` would improve cohesion.

The important lesson is that new files should be created only when they represent a meaningful responsibility, not simply because a new function exists.

---

## Biggest takeaway

Software engineering is about making good design decisions, not blindly following rules.

The goal is to keep responsibilities clear, dependencies small and related code grouped together while making reasonable tradeoffs.