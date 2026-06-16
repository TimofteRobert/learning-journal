# Day 7 - Service Layer and Database Improvements

## Topics learned

Today I refactored the FastAPI project by adding a service layer.

The new architecture:

Client
↓
FastAPI Router
↓
Service Layer
↓
Database


## Service Layer

The service layer separates business/database logic from the routes.

The route should handle:
- HTTP requests
- validation
- returning responses

The service should handle:
- database operations
- application logic
- transactions


## Separation of concerns

Keeping different responsibilities in different files improves:

- readability
- maintenance
- scalability
- reducing duplicated code
- easier testing


## Database error handling

Learned how to use:

try:
- execute database operations

except:
- handle errors
- rollback changes

finally:
- always close the database connection


## Transactions

A transaction means database changes are treated as one operation.

commit():
- saves changes permanently

rollback():
- cancels changes if something fails


## CRUD service functions

Created service functions for:

- Create note
- Read notes
- Read note by id
- Update note
- Delete note


## Important discovery

UPDATE and DELETE do not automatically return an error if an id does not exist.

Used:

cur.rowcount

to check if a database operation actually affected a row.


## Problems fixed

- Moved database code away from routes
- Added safer database connection handling
- Improved project structure