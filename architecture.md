# Architecture

## 1) Overview

The Nursery Management App is a full-stack system built with:

- **Frontend:** React (SPA) + React Router + Context API
- **Backend:** Node.js + Express (REST API)
- **Database:** MongoDB (Mongoose models)
- **Auth:** JWT authentication + role-based authorization

The system supports three roles:

- Parent
- Employee
- Admin

---

## 2) High-Level Architecture

```text
[ React Frontend ] --Axios/HTTP--> [ Express REST API ] --Mongoose--> [ MongoDB ]
        |                                |
        |                                +-> Services (billing/pdf/mail/notifications)
        |
        +-> Role-based UI + Protected routing
```

---

## 3) Frontend Design (`frontend/src`)

### 3.1 Key directories

- `pages/`
  - Public pages: `HomePage`, `LoginPage`, `NotificationsPage`
  - Role pages:
    - `pages/parent/*`
    - `pages/employee/*`
    - `pages/admin/*`
- `components/`
  - Global UI: `Navbar`, `Footer`, `Banner`, `ImageGrid`
  - `ProtectedRoute` for route security
  - Form components in `components/forms/*`
  - Notifications UI in `components/notifications/*`
- `context/`
  - `AuthContext` – stores logged-in user + token, login/logout logic
  - `UserRoleContext` – role helper state (optional)
  - `NotificationContext` – stores notification list/unread count and helper actions
- `utils/api.js`
  - Axios instance
  - Adds `Authorization` header for protected requests
  - Exposes helper functions for calling the backend
- `router/AppRouter.jsx`
  - Defines all routes and redirects
  - Uses `ProtectedRoute requiredRole="..."` to enforce roles

### 3.2 Frontend authentication flow

1. User submits login form
2. React calls `POST /api/auth/login`
3. Backend returns token + user
4. Frontend stores token and user (`AuthContext`)
5. `AppRouter` redirects based on role

---

## 4) Backend Design (`backend/`)

### 4.1 Key directories

- `server.js`
  - Express server setup
  - Connects to MongoDB
  - Registers route modules under `/api`
- `config/db.js`
  - MongoDB connection using Mongoose (comment should reflect MongoDB, not SQL)
- `routes/`
  - Defines endpoints by domain:
    - `auth`, `parent`, `employee`, `admin`
    - `attendance`, `activity`, `payment`, `notification`
- `controllers/`
  - Business logic for each route group
  - Validates input, calls models/services, returns JSON
- `models/`
  - Mongoose schemas: `User`, `Child`, `Attendance`, `Activity`, `Payment`, `Notification`, `MedicalAlert`
- `middleware/`
  - `authMiddleware` verifies JWT and attaches `req.user`
  - `roleMiddleware` checks role permissions
  - `errorHandler` standardizes error responses
- `services/`
  - `billingService`: invoice calculations
  - `pdfService`: invoice PDF generation
  - `notificationService`: create in-app notifications and optionally send emails
  - `mailService`: email delivery

### 4.2 Typical backend request flow

Request → Route → `authMiddleware` → `roleMiddleware` → Controller → Model/Service → Response

---

## 5) Key Business Flows

### 5.1 Notifications

Notifications are saved in MongoDB and displayed in `/notifications`.

Created when:

- A medical alert is reported
- A payment is created/updated/reminded
- An activity/note is added (if implemented)

Retrieved from:

- `GET /api/notifications/my-notifications`

### 5.2 Medical Alerts (Employee → Parent + Admin)

- Employee creates an alert from `/employee/medical-alerts`
- Frontend sends `POST /api/employee/medical-alerts`
- Backend:
  - Finds employee classroom
  - Creates `MedicalAlert` with classroom + child
  - Creates `Notification` for:
    - the child’s parent
    - all admins
- Employee can list alerts via `GET /api/employee/medical-alerts`
- Employee can resolve via `PATCH /api/employee/medical-alerts/:id/resolve`

### 5.3 Payments

- Payments are created on child registration (if implemented)
- Parents see payments in `/parent/payments`
- Admins manage payments in `/admin/manage-payments`
- Payment updates can generate notifications

---

## 6) Security and Data Access Rules

- Parents can only access their own children and related records.
- Employees can only access children and alerts in their assigned classroom.
- Admins can access all data.

This is enforced by:

- Backend scoped queries
- RBAC middleware
- Frontend protected routes