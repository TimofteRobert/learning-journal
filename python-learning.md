## FastAPI architecture

Typical request flow:

HTML
→ script.js
→ api.js
→ FastAPI Route
→ Service
→ Database
→ Service
→ Route
→ api.js
→ script.js
→ HTML

Responsibilities:

- HTML
  Displays the interface.

- script.js
  Reads user input and updates the page.

- api.js
  Sends HTTP requests.

- Route
  Receives requests and forwards them.

- Service
  Contains business logic and SQL.

- Database
  Stores the data.

Debugging rule:

Always identify which layer is failing before changing code.

## Day 29

### Good Helper Functions

A helper function is useful when it:

- hides complexity,
- represents a meaningful concept,
- reduces duplicated logic,
- centralizes future changes.

Example:

```python
def make_note(note_id, title, content):
    return {
        "id": note_id,
        "title": title,
        "content": content
    }
```

### Not Every Repeated Line Should Become a Helper

Some repeated code is too small to justify abstraction.

Example:

```python
except Exception:
    conn.rollback()
    raise
```

Creating a helper only for `rollback()` would not improve readability.

### Data Flow Reminder

Frontend

↓

script.js

↓

api.js

↓

FastAPI Route

↓

Service Layer

↓

Database

↓

Service Layer

↓

Route

↓

api.js

↓

script.js

↓

Frontend

### Adding New Data

When adding a new field (for example `author` or `created_at`):

1. Update the SQL query.
2. Update the object construction (`make_note()`).
3. Update models/routes if necessary.
4. Display the new field in the frontend.

The data should always originate from the database and flow through each application layer.