# Day 52 — Task Manager: models, relationships, and schemas

## What we built
- Scaffolded task-manager repo (FastAPI, SQLAlchemy, Pydantic, Ruff, venv) — github.com/TimofteRobert/task-manager
- `app/database.py`: engine, SessionLocal, Base, `get_db()` dependency (per-request session, always closed via try/finally)
- `app/models.py`: `Project` 1—many `Task`
- `app/schemas.py`: Create/Read schemas per table

## Key concepts

### Foreign keys — the core lesson of this project
- The FK column lives on the **"many" side**: `tasks.project_id`, NOT on projects.
- It is NOT a primary key. `tasks.id` is the PK; `project_id` is a plain int column whose
  **value is a copy of a project's PK**. That copied value is the connection.
- Rule of thumb: **the FK goes on the table that *belongs to* the other one.**
- `ForeignKey("projects.id", ondelete="CASCADE")` → DB-level rule: delete a project, its tasks die too.
- `cascade="all, delete-orphan"` on `Project.tasks` → the ORM-level version of the same rule.
- Both exist because SQLAlchemy cascade only fires on ORM deletes; direct SQL deletes need `ondelete`.

### Relationships in SQLAlchemy
- `relationship()` on both sides, linked by `back_populates` (each names the other's attribute).
- `Project.tasks` is `Mapped[list["Task"]]` — one side holds a list.
- `Task.project` is `Mapped["Project"]` — many side holds a single object.

### Pydantic schemas — why Create/Read are separate
- `ProjectCreate`/`TaskCreate`: what the client sends (no id, no created_at — server owns those).
- `*Read`: what we return. `model_config = ConfigDict(from_attributes=True)` lets Pydantic read
  attributes off SQLAlchemy objects, not just dicts.
- `ProjectRead` has `tasks: list[TaskRead]`; `TaskRead` has `project_id: int`.
  Each side of the relationship exposes what it naturally knows: a project knows its many tasks,
  a task knows its one project's id.
- Forward reference: `ProjectRead` mentions `"TaskRead"` before definition → needs `ProjectRead.model_rebuild()`.

### Tools & workflow
- PEP 668: Ubuntu blocks system-wide pip → venv is the correct answer anyway (`python3 -m venv .venv`).
- `pip freeze > requirements.txt` — commit it; never commit `.venv/` (gitignore it).
- Ruff = linter + formatter for Python (Prettier's equivalent, but for Python). `ruff check app --fix`,
  then `ruff format app`. E501 long lines fixed by hand → multi-line call style.
- `.gitignore` should NOT ignore itself. Already-tracked files stay tracked even after adding
  ignore rules → `git rm -r --cached <path>` to untrack without deleting.
- `F401` can be a false positive for side-effect imports (`from app import models  # noqa: F401`).
- One SSH key per machine, not per project. `-C` in ssh-keygen is just a label.
- `uvicorn app.main:app` must run from the project root (import paths are relative to CWD).

## Verified today
- `.schema tasks` in sqlite3 shows `FOREIGN KEY(project_id) REFERENCES projects (id) ON DELETE CASCADE`
- Swagger UI works at /docs, root endpoint returns JSON

## Tomorrow
- Routes: projects CRUD + nested `/projects/{id}/tasks`
- Hand-test: create project → add 2 tasks → list → delete project → verify cascade in the DB

## Still fuzzy (revisit)
- Difference between SQLAlchemy-level cascade and `ondelete` — when does each one actually fire?
- Why `ProjectRead` needed `model_rebuild()` — forward references in Pydantic