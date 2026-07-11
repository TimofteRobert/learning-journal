# Day 27 – Search Statistics

## What I implemented
- Added backend support for note statistics with search.
- `get_note_stats(search)` now returns:
  - `total_notes`
  - `matching_notes`
- Matching notes are filtered using:
  - `title ILIKE %search%`
  - `content ILIKE %search%`

## Backend flow
Frontend
→ api.js
→ notes.py (route)
→ notes_service.py
→ PostgreSQL
→ response back to frontend

## Frontend changes
### api.js
- Updated `getNoteStats(search)` to send:
  `/notes/stats?search=...`

### script.js
- `loadStats()` now:
  - reads the search box
  - calls `getNoteStats(search)`
  - displays statistics with `innerHTML`

### index.html
Changed:

```html
oninput="loadNotes()"
```

to

```html
oninput="refreshPage()"
```

so that typing updates:
- notes
- statistics

at the same time.

## Bugs found and fixed
Bug:
- Statistics always equaled total notes.

Reason:
- Only `loadNotes()` was called while typing.
- `loadStats()` never ran.

Fix:
- Call `refreshPage()` instead.

## Debugging lesson
Used Swagger to verify:
- Backend worked correctly.

Used Firefox Network tab to verify:
- Requests sent by the frontend.

Instead of guessing, isolated the problem by checking each layer:
Frontend
→ API
→ Route
→ Service
→ Database

## Biggest lesson today
When debugging, first determine **which layer is failing** instead of immediately assuming the SQL or backend is wrong.