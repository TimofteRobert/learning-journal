# Day 17 - Search and Sorting Notes

## Topics learned

Today I added search and sorting functionality to the Notes application.

The goal was to allow users to:
- search notes by title or content
- sort notes by different options


## Query parameters

The frontend now sends additional data to the backend.

Example:
/notes?search=test&sort=title


The backend receives:

- search = test
- sort = title


## Frontend flow

The sorting value comes from the dropdown:

HTML
↓
script.js
↓
api.js
↓
FastAPI route
↓
service layer
↓
database


## FastAPI route changes

The notes route now accepts:

```python
search: Optional[str] = None
sort: str = "id"

The route passes these values to the service layer.

Service layer sorting

The service layer maps allowed sorting options:
sort_options = {
    "id": "id ASC",
    "id_desc": "id DESC",
    "title": "title ASC",
    "title_desc": "title DESC"
}

This prevents unsafe SQL generation because only predefined values can become SQL.

SQL ORDER BY

The database now receives an ORDER BY clause.

Examples:
ORDER BY id ASC
or:

ORDER BY title DESC
Security learning

User input should not directly become SQL code.

Instead of:

ORDER BY {user_input}

we use a controlled mapping.

Current project status

Frontend:
    CRUD interface
    validation
    error handling
    search
    sorting

Backend:
    FastAPI routes
    service layer
    PostgreSQL
    query parameters

Infrastructure:
    Docker
    environment variables

Next step

Continue improving backend structure and learn more about organizing database queries and services.