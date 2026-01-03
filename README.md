 
# Nursery Management App – Documentation

This folder contains the complete documentation for the **Nursery Management App** (React + Node.js/Express + MongoDB).

## Documentation Index

### 1) Setup & Running
- **[Setup Guide](./setup.md)** – install dependencies, configure `.env`, run frontend/backend
- **[Troubleshooting](./troubleshooting.md)** – common issues and solutions

### 2) System Design
- **[Architecture](./architecture.md)** – how the frontend, backend, and database work together
- **[Database](./database.md)** – MongoDB collections/models, relationships, and indexing notes
- **[Roles & Permissions](./roles-and-permissions.md)** – what each role can access and do

### 3) API
- **[API Reference](./api.md)** – main endpoints grouped by role

### 4) User Guide (End Users)
- **[User Guide Index](./user-guide/README.md)**
  - [Getting Started](./user-guide/getting-started.md)
  - [Parent Guide](./user-guide/parent.md)
  - [Employee Guide](./user-guide/employee.md)
  - [Admin Guide](./user-guide/admin.md)

---

## Project Summary

The Nursery Management App is a role‑based web system for managing nursery/daycare operations:

- Parent portal (children tracking, payments, notifications)
- Employee portal (attendance, activities, notes, medical alerts)
- Admin portal (user management, classrooms, payments, reports, capacity)
- Central notifications page for all roles

---

## Where things live

- **Frontend:** `frontend/`
- **Backend:** `backend/`
- **Models (MongoDB/Mongoose):** `backend/models/`
- **Routes:** `backend/routes/`
- **Controllers:** `backend/controllers/`

---

## Notes

- API base URL in development is usually: `http://localhost:5000/api`
- If you change any `.env` values, restart the related server (frontend or backend).