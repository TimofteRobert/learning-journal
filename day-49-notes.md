# Day 49 - Separate Create and Update Schemas

## Overview

Today I improved the note update flow by separating the data required to create a note from the data allowed when updating one.

Previously, both creating and updating a note used the `NoteCreate` model. This meant the update endpoint technically accepted `user_id`, even though updating a note should not change its owner.

The API was refactored so creation and updating now have separate schemas.

## 1. Created the `NoteUpdate` model

A new Pydantic model was added:

```python
class NoteUpdate(BaseModel):
    title: str = Field(min_length=3)
    content: str = Field(min_length=5)
```

`NoteUpdate` contains only the fields that are allowed to change.

`user_id` is deliberately excluded.

## 2. Updated the PUT route

The update route was changed from:

```python
def update_note_route(note_id: int, note: NoteCreate):
```

to:

```python
def update_note_route(note_id: int, note: NoteUpdate):
```

The Swagger documentation now shows only `title` and `content` in the request body for `PUT /notes/{note_id}`.

The response still contains the complete note, including:

* `id`
* `title`
* `content`
* `created_at`
* `user_id`

This is intentional because the response represents the complete note, while the request represents only the fields that can be modified.

## 3. Updated the service layer

The note service now uses the appropriate model for each operation:

```python
def create_note(note: NoteCreate):
```

and:

```python
def update_note(note_id: int, note: NoteUpdate):
```

The SQL used by `update_note()` already updated only:

```sql
UPDATE notes
SET title = %s,
    content = %s
WHERE id = %s
```

Therefore, the existing `user_id` remains unchanged.

## 4. Updated the frontend

The frontend update operation previously passed the complete note object returned by the form validation.

A separate object is now created for updates:

```javascript
const updateData = {
    title: note.title,
    content: note.content
};

await updateNoteApi(editingNoteId, updateData);
```

The frontend therefore matches the new API contract and does not send `user_id` when updating a note.

## 5. Improved the owner selector UX

While editing a note, the owner selector is now disabled.

When editing starts:

```javascript
document.getElementById("user").disabled = true;
```

When the form is cleared:

```javascript
document.getElementById("user").disabled = false;
```

This prevents the interface from suggesting that the owner can be changed during an update when the API deliberately does not allow it.

## 6. Testing

The update flow was tested successfully.

I tested changing the title and content of an existing note and confirmed that the update works.

I also tested selecting a different owner while editing. The owner remained unchanged, confirming that the update operation only modifies the fields that are intended to be updated.

Validation for the minimum title and content lengths was also tested.

The API was checked through Swagger, where the PUT request body correctly contains only:

```json
{
    "title": "...",
    "content": "..."
}
```

## Key takeaway

Create and update operations do not necessarily need to use the same schema.

Using separate models makes the API contract clearer and prevents clients from modifying fields that should remain unchanged.

The current flow is:

```text
POST /notes
    ↓
NoteCreate
    ↓
title + content + user_id
    ↓
Create note


PUT /notes/{id}
    ↓
NoteUpdate
    ↓
title + content
    ↓
Update only editable fields
    ↓
Existing user_id preserved
```

## Git

Project changes were committed and pushed with:

```text
refactor: separate note update schema
```

The tracked `__pycache__` files remain a separate cleanup issue and were intentionally not included in this feature commit.
