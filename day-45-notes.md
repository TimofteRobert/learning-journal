# Day 45 — Database Constraints and Alembic Migrations

## What I learned

Today we continued improving the database side of the `hello-fastapi` project.

The main focus was **database constraints and database migrations with Alembic**.

---

## Database constraints

The `notes` table already has these constraints:

```sql
CHECK (length(title) >= 3)
CHECK (length(content) >= 5)
```

These enforce minimum lengths directly at the PostgreSQL database level.

This gives us two layers of validation:

* **Pydantic/FastAPI** validates incoming API requests.
* **PostgreSQL** protects the database itself.

For example, attempting to insert:

```sql
INSERT INTO notes (title, content)
VALUES ('a', 'abc');
```

fails because the data violates the database constraints.

A valid insert succeeds.

This is important because application-level validation should not be the only thing protecting the database.

---

## Checking the database directly

PostgreSQL can be accessed through `psql` inside the Docker container:

```bash
docker exec -it hello-fastapi-postgres-1 psql -U appuser -d notesdb
```

Inside `psql`, commands such as:

```sql
\d notes
```

show the structure of the `notes` table.

We also used:

```sql
SELECT * FROM alembic_version;
```

to see which Alembic migration the database is currently using.

---

# Alembic

Alembic is a database migration tool commonly used with Python applications.

It allows us to keep a history of **database schema changes** in the repository.

We installed/verified Alembic and initialised it with:

```bash
alembic init alembic
```

This created the Alembic directory and configuration files.

The important directory is:

```text
alembic/
└── versions/
```

Migration files are stored inside `versions`.

---

## Connecting Alembic to PostgreSQL

We modified:

```text
alembic/env.py
```

so that it loads the database settings from `.env` and constructs the PostgreSQL connection URL.

The project already uses environment variables such as:

```text
DB_HOST
DB_PORT
DB_NAME
DB_USER
DB_PASSWORD
```

The password remains in `.env`, which is ignored by Git.

---

## The baseline migration

We created:

```text
0a872695f466_baseline_existing_database.py
```

This represents the point at which Alembic began tracking the already-existing database.

Its `upgrade()` and `downgrade()` functions were initially empty because the database already existed.

We then used:

```bash
alembic stamp head
```

to tell Alembic that the existing database should be considered to be at that migration without actually executing the migration.

The database then reported:

```text
0a872695f466 (head)
```

---

## Creating migrations

We created a migration:

```text
f3f335edca7f_add_description_to_notes.py
```

which added a `description` column.

Its upgrade operation was:

```python
op.add_column(
    "notes",
    sa.Column("description", sa.String("TEXT"))
)
```

and its downgrade operation removed the column:

```python
op.drop_column("notes", "description")
```

We then created another migration:

```text
9f4c9fa19976_add_category_to_notes.py
```

which added a `category` column.

Its upgrade operation was:

```python
op.add_column("notes", sa.Column("category", sa.Text()))
```

and its downgrade operation was:

```python
op.drop_column("notes", "category")
```

---

## Migration history

Our migration chain became:

```text
<base>
   ↓
0a872695f466
   ↓
f3f335edca7f
   ↓
9f4c9fa19976
```

The latest migration is called the **head**.

We can inspect the history with:

```bash
alembic history
```

and check the database's current migration with:

```bash
alembic current
```

---

## Upgrading

We used:

```bash
alembic upgrade head
```

This tells Alembic to execute all migrations necessary to bring the database to the latest migration in the repository.

For example:

```text
f3f335edca7f
       ↓
9f4c9fa19976
```

caused the `category` column to be added.

---

## Downgrading

We also tested:

```bash
alembic downgrade f3f335edca7f
```

This reversed the migration from:

```text
9f4c9fa19976
```

to:

```text
f3f335edca7f
```

The `category` column disappeared while `description` remained.

We then upgraded back to the latest migration.

This demonstrated that migrations are not just a list of changes: they normally contain both an **upgrade** and a **downgrade** operation.

---

## Important concept: Alembic does not store a copy of the database

A migration is not a backup of the database.

It is a set of instructions describing how to change the **database schema**.

For example:

```text
Migration A
    ↓
create table

Migration B
    ↓
add description

Migration C
    ↓
add category
```

`alembic upgrade head` executes the required instructions until the database reaches the latest migration.

The actual data in the database is a separate matter.

---

## `alembic_version`

Alembic keeps track of the database's current migration in:

```text
alembic_version
```

For example:

```sql
SELECT * FROM alembic_version;
```

can return:

```text
version_num
--------------
f3f335edca7f
```

This tells Alembic where the database currently is in the migration chain.

---

## Important discovery

We attempted:

```bash
alembic revision --autogenerate -m "test schema detection"
```

and received an error because:

```python
target_metadata = None
```

Alembic's autogeneration feature needs SQLAlchemy metadata describing the application's database models so it can compare the expected schema with the actual database schema.

Our project currently uses `psycopg` directly rather than SQLAlchemy ORM models, so we cannot simply enable autogeneration yet.

This was useful because it showed us an important distinction:

* **manual migrations** tell Alembic exactly what change to make;
* **autogeneration** can detect differences when Alembic has appropriate model metadata to compare against the database.

---

## Important correction to my mental model

I initially thought:

> `alembic upgrade head` might be similar to `git pull`.

The analogy is useful, but not exact.

Git brings repository changes to the local machine.

Alembic executes migration instructions to change a database schema.

A better simplified model is:

```text
Git:
Repository → working copy

Alembic:
Migration history → database schema
```

Also, `head` is not manually chosen by the developer running the command. It refers to the latest revision in the migration chain currently present in the repository.

---

## Current migration state

At the end of today's work, the migration history is:

```text
<base>
   ↓
0a872695f466 — baseline existing database
   ↓
f3f335edca7f — add description to notes
   ↓
9f4c9fa19976 — add category to notes
```

The database was tested with both upgrade and downgrade operations.

---

## Important next step

There is one issue that we deliberately left for the next session.

The baseline migration contains:

```python
def upgrade():
    pass
```

Therefore, a completely fresh database cannot currently be recreated from the migration history alone.

The next session should address this and make the database schema properly reproducible for another developer/environment.

---

## Practical developer habits reinforced today

* Use migrations instead of manually changing production database schemas.
* Keep migration files in Git.
* Use `alembic current` to check database migration state.
* Use `alembic history` to inspect migration history.
* Test both upgrades and downgrades.
* Keep database credentials in environment variables rather than source control.
* Understand what generated commands actually do instead of blindly running them.
