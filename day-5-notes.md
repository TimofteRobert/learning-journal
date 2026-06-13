# Day 5 - Service Layer Introduction

## What I learned today
- Introduced service layer in FastAPI
- Refactored CRUD operations into services
- Separated routes from database logic

## Architecture evolution
Before:
- Routes directly handled SQL queries

After:
- Routes → Services → Database

## Key concepts
- Separation of concerns
- Clean architecture basics
- Refactoring without changing behavior

## Backend structure
- routes: handle HTTP requests
- services: contain business logic + DB operations
- models: validate data (Pydantic)
- database: connection management

## Technical challenges solved
- Fixed PostgreSQL connection issue (Docker not running)
- Fixed 422 error caused by incorrect decorators
- Successfully refactored POST/PUT/DELETE into service layer

## Key takeaway
Backend development is not just writing endpoints, but structuring code so it scales and stays maintainable.