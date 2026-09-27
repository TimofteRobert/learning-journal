# Day 53 — Task Manager: CRUD routes (Day 2 of project)

## What we built
- Full CRUD API: `app/routes/projects.py` + `app/routes/tasks.py`
- Nested task routes: `/projects/{project_id}/tasks`
- All hand-tested in Swagger: create, list, get, 404s, delete

## Key concepts

### Routers
- `APIRouter(prefix=..., tags=[...])` = a mini-app for one resource; `main.py` plugs it in
  with `app.include_router(projects.router)`. Keeps main.py as pure wiring.
- Import errors are literal: "module X has no attribute Y" means exactly that —
  an empty `tasks.py` has no `router` variable yet.

### Dependency injection (the big one today)
- `db: Session = Depends(get_db)` — FastAPI runs `get_db` before the endpoint and
  closes the session after, via the `try/finally` in database.py. The endpoint
  *receives* a session instead of creating one. That's dependency injection.
- `response_model=ProjectRead` — FastAPI converts the returned SQLAlchemy object
  through the schema (this is where `from_attributes=True` matters). Output fields
  are filtered to what the schema declares.

### The endpoint patterns (memorize the shape, not the words)
- Create: `Model(**schema.model_dump())` → add, commit, refresh → return
- Get one: `db.get(Model, id)` returns None if missing → check → 404 via HTTPException
- Update: fetch → `for field, value in schema.model_dump().items(): setattr(obj, field, value)`
  → commit, refresh
- Delete: fetch → `db.delete(obj)` → commit → 204 (no response body — correct, not broken)

### Nested routes & the parent check
- Prefix `/projects/{project_id}/tasks` — every task endpoint receives the parent id.
- `get_project_or_404()` helper = DRY: the same 4-line check extracted once.
- `Task(**task.model_dump(), project=db_project)` — attach via the relationship object;
  SQLAlchemy fills project_id itself.
- The subtle check: `db_task.project_id != project_id` → 404. A task is only reachable
  through its own project. `/projects/2/tasks/5` where task 5 belongs to project 1 = 404,
  even though the task exists. Correct REST, not a bug.

### Workflow (new daily routine)
- Start of day: `git status` → `git pull` → activate venv → verify server runs
- End of day: commit → push → journal. GitHub = source of truth across my two machines.
- Ruff loop before every commit: `ruff check app --fix && ruff format app`

## Verified in Swagger
- POST/GET/PUT/DELETE projects and tasks all behave
- 404s fire for missing project, missing task, and task-under-wrong-project

## Tomorrow
- DBeaver: fix "database is locked" (SQLite = single writer; stop server or open read-only)
  and visually verify cascade delete in the tasks table
- Start thinking about the frontend structure (api.js for fetch calls)

## Still fuzzy (revisit)
- When exactly does SQLAlchemy cascade fire vs `ondelete` — never saw the raw table state
- Why `db.refresh()` is needed after commit (DB generates id/created_at; refresh reloads them)