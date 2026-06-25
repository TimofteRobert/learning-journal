# Day 14 - Frontend Validation and Request States

## Topics learned

Today I improved the frontend reliability by adding validation before sending requests to the backend.

The goal was to prevent invalid data from reaching FastAPI.

## Frontend validation

Added:

```javascript
validateNote()

This checks:

title is not empty
content is not empty
title has minimum length
content has minimum length

Benefits:

faster feedback
fewer unnecessary API requests
better user experience
Defense in depth

The application now validates data in multiple layers:

Frontend
↓
FastAPI / Pydantic
↓
Database constraints

Each layer protects the application.

Request state handling

Added button disabling during API operations.

Example:

User clicks create:

Button disabled
↓
Request sent
↓
Response received
↓
Button enabled

This prevents duplicate requests.

finally block

Learned that JavaScript has:

try
catch
finally

The finally block runs whether the operation succeeds or fails.

Used it to restore button states.

Code organization

The frontend now contains:

UI helpers
validation functions
error handling
rendering functions
API communication
CRUD operations
Current frontend architecture

HTML
↓
script.js
(UI logic)
↓
api.js
(API communication)
↓
FastAPI
↓
Service layer
↓
PostgreSQL