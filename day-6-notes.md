Day 6 — Database Connection, Docker & Debugging

Today I worked on connecting FastAPI to PostgreSQL using Docker.

What I learned:
FastAPI only works if the database service is running
Docker containers must be started manually using docker compose up -d
Environment variables must be correctly formatted in .env
Small mistakes in .env can break database connections completely

Debugging experience:
Fixed psycopg OperationalError caused by incorrect .env formatting
Learned how to interpret stack traces step by step
Understood difference between application errors and infrastructure errors

Key concepts:
Service architecture (FastAPI → Service layer → Database)
Docker as a runtime environment
Separation between code and infrastructure
Importance of logs for debugging

Outcome:
Full CRUD API working with PostgreSQL
Docker-based database successfully integrated
Improved debugging workflow for backend systems