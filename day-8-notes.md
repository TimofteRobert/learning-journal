# Day 8 - Frontend Integration

## Topics learned

Today I connected a frontend with the FastAPI backend.

Architecture:

Browser
↓
JavaScript
↓
FastAPI REST API
↓
Service Layer
↓
PostgreSQL


## Frontend basics

Created:
- index.html
- script.js
- style.css

Learned:
- HTML structure
- JavaScript functions connected to buttons
- DOM manipulation


## Fetch API

Used JavaScript fetch() to send HTTP requests.

Created POST request:

POST /notes

The frontend sends JSON data:

{
"title": "...",
"content": "..."
}


## Async JavaScript

Learned:
- async functions
- await keyword
- waiting for API responses


## CORS

Learned why browsers block requests between different origins.

Added FastAPI CORSMiddleware to allow frontend communication.


## Full-stack flow

User action:
↓
JavaScript
↓
HTTP request
↓
FastAPI
↓
Database
↓
JSON response
↓
Frontend display


## Important concept

Backend provides data.
Frontend decides how the data is displayed.