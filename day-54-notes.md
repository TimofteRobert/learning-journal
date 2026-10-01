# Day 54 — Task Manager: frontend + CORS (Day 3 of project)

## What we built
- Frontend skeleton: `index.html`, `css/styles.css`, `js/api.js`, `js/main.js`
- First full-stack moment: browser → api.js → FastAPI → SQLite → rendered page

## Key concepts

### Serving a frontend over HTTP
- `python3 -m http.server 5500` from the frontend folder. A server maps URLs to files;
  opening `/` serves index.html only if the server runs in its folder. Directory listing
  = server is fine, wrong folder.
- `file://` doesn't support ES modules or fetch — that's why the page must be served over HTTP.
- Two servers running at once is normal: uvicorn (API, :8000) + http server (pages, :5500).

### CORS — the server decides who may call it
- Browser blocks pages on one origin from reading responses of another origin unless
  the API explicitly allows it (Same Origin Policy).
- Fix lives in the API: `app.add_middleware(CORSMiddleware, allow_origins=["http://localhost:5500"], ...)`.
- Saw it broken (commented out middleware → fetch blocked in DevTools) and fixed.
- Specific origin, not `["*"]` — real APIs don't allow everything; `*` breaks with auth later.
- Server = gatekeeper. The frontend cannot "opt in" to another origin's data; the API grants permission.

### api.js — the service layer pattern
- One file owns ALL backend communication. Data problem → api.js; display problem → UI files.
- `request()` helper = DRY: shared URL base, headers, error handling, 204 handling.
- `fetch` gotchas: doesn't throw on 404 by default — check `response.ok` / `.status` yourself.
  204 responses have no body — `response.json()` would crash, return null instead.
- `async/await` = syntax for waiting for network responses (deeper dive later).

### Small lessons
- `ps aux   grep x` always shows grep itself — ignore that line; its PID dies instantly.
- SQL printing nothing = empty result, not an error. Empty success looks identical to failure.
- sqlite3 read-only: `sqlite3 "file:taskmanager.db?mode=ro" "SELECT * FROM tasks;"` — no locks.
- DBeaver `open_mode` is a numeric bitmask, not a word ("Read-Only" → Invalid argument).

## Verified today
- Project list renders real API data: names + task counts from the `ProjectRead.tasks` relationship
- CORS error reproduced (middleware commented out) then fixed

## Tomorrow
- Create-project form (POST from the page)
- Visual design pass — the goal is "noticeably better UI than the Notes App"

## Still fuzzy (revisit)
- How `async/await` actually works under the hood
- Why DBeaver kept saying SQLITE_BUSY even with the server stopped