 

```markdown name=docs/roles-and-permissions.md
# Roles & Permissions

The app supports three roles:
- **Parent**
- **Employee**
- **Admin**

Access control is implemented in two layers:
1. **Frontend:** protected routes using `ProtectedRoute`
2. **Backend:** JWT verification + role checks via middleware and scoped queries

---

## 1) Parent

### Allowed pages (frontend)
- `/parent/dashboard`
- `/parent/track-child`
- `/parent/register-child`
- `/parent/messages`
- `/parent/profile`
- `/parent/payments`
- `/parent/daily-report`
- `/notifications`

### Allowed actions
- View/manage own children
- View attendance/activities/reports for own children
- View and pay invoices
- Read notifications

### Data scope rules
- Parent can only access children where `child.parent == req.user.id`.

---

## 2) Employee

### Allowed pages (frontend)
- `/employee/dashboard`
- `/employee/attendance`
- `/employee/activities`
- `/employee/child-notes`
- `/employee/profile`
- `/employee/medical-alerts`
- `/notifications`

### Allowed actions
- View assigned classroom and children
- Record attendance
- Create activities and notes for classroom children
- Report & resolve medical alerts
- Trigger notifications (parents/admins) via actions

### Data scope rules
- Employee can only access children in their classroom.
- Employee can only access medical alerts where `alert.classroom == employee.classroom`.

---

## 3) Admin

### Allowed pages (frontend)
- `/admin/dashboard`
- `/admin/manage-parents`
- `/admin/manage-employees`
- `/admin/manage-children`
- `/admin/manage-payments`
- `/admin/manage-classes`
- `/admin/reports`
- `/admin/capacity`
- `/notifications`

### Allowed actions
- Full management of users, children, classes
- Full management of payments
- View dashboards and reports
- Receive high-priority alerts and notifications

### Data scope rules
- Admin can access all data.

---

## 4) Enforcement Details

### Frontend enforcement
`frontend/src/components/ProtectedRoute.jsx` should:
- Redirect to `/login` if not authenticated
- Redirect or block if role is not allowed

### Backend enforcement
- `backend/middleware/authMiddleware.js`
  - Validates JWT and sets `req.user` (id, role)
- `backend/middleware/roleMiddleware.js`
  - Ensures role matches endpoint requirements

---

## 5) Recommended Status Codes

- `401 Unauthorized` → missing/invalid token
- `403 Forbidden` → authenticated but wrong role
- `404 Not Found` → resource not found or not accessible in your scope
- `400 Bad Request` → validation error