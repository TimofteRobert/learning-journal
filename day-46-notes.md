# Day 46 — Making the Database Reproducible with Alembic

## Goal

Make sure a completely fresh PostgreSQL database can be created from the project's Alembic migration history.

## What I learned

### 1. Why the Alembic baseline matters

The original Alembic baseline migration was:

```python
def upgrade() -> None:
    pass

def downgrade() -> None:
    pass
```

This worked for the existing database because the database already contained the `notes` table and we used `alembic stamp` to tell Alembic that the database was already at the baseline revision.

However, a completely new database would start at `base` and execute the migrations from the beginning.

Because the baseline did nothing, the next migration would try to add `description` to a `notes` table that did not exist.

Therefore, the baseline needed to describe the original schema.

### 2. What the baseline migration represents

The baseline represents the database schema that existed before Alembic was introduced.

The original `notes` table contains:

* `id` — integer primary key with auto-increment
* `title` — text, not null
* `content` — text, not null
* `created_at` — timestamp, not null, with `CURRENT_TIMESTAMP` as the database-side default
* `notes_title_min_length` — title must contain at least 3 characters
* `notes_content_min_length` — content must contain at least 5 characters

The later migrations are responsible for later changes:

```text
0a872695f466
    ↓
Create original notes table
    ↓
f3f335edca7f
    ↓
Add description
    ↓
9f4c9fa19976
    ↓
Add category
```

### 3. `create_table()` vs `add_column()`

I learned that these operations have different purposes.

`op.create_table()` is used when the table itself needs to be created.

`op.add_column()` is used when the table already exists and a new column needs to be added.

For example, the baseline uses:

```python
op.create_table(...)
```

while the later `description` migration uses:

```python
op.add_column(...)
```

### 4. Database-side defaults

For `created_at`, the migration uses:

```python
server_default=sa.text("CURRENT_TIMESTAMP")
```

This means PostgreSQL generates the timestamp when a row is inserted.

This is different from a Python-side `default`.

### 5. Existing database vs fresh database

I learned that an existing database and a fresh database can have different starting points in the migration process.

The existing database was already created before Alembic existed, so we stamped it at the appropriate baseline revision instead of recreating the existing schema.

A fresh database has no schema, so Alembic must execute every migration from `base` to `head`.

### 6. Testing reproducibility safely

Instead of deleting my real `notesdb`, I created a separate temporary database called `notesdb_fresh`.

I temporarily changed `.env` to point Alembic at that database and ran:

```bash
alembic upgrade head
```

Alembic successfully executed:

```text
base
↓
0a872695f466
↓
f3f335edca7f
↓
9f4c9fa19976
```

The resulting database contained the complete `notes` schema, including:

* `id`
* `title`
* `content`
* `created_at`
* `description`
* `category`
* primary key
* original check constraints

The `alembic_version` table contained:

```text
9f4c9fa19976
```

This proved that the migration history can reproduce the database schema from an empty database.

The temporary database was then deleted and `.env` was restored to `notesdb`.

## Important real-world lesson

In a real project, already-published migrations should generally not be rewritten because other developers, environments, or deployments may already depend on them.

For this learning project, I modified the baseline because I was establishing the correct historical baseline and testing how to make the project reproducible.

For future schema changes, the normal approach is to create a new migration rather than rewrite migration history.

## Result

The Notes API database is now reproducible through Alembic:

```text
Fresh PostgreSQL database
        ↓
alembic upgrade head
        ↓
Complete database schema
```

The existing database was not destroyed or recreated during the test.
