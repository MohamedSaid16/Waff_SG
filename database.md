 

```markdown name=docs/database.md
# Database Documentation (MongoDB)

The backend uses **MongoDB** with **Mongoose**.

Models are defined in:
- `backend/models/`

This document describes the main collections and how they relate.

---

## 1) User Collection (`User`)

Purpose: store all users (parents, employees, admins).

Typical fields:
- `firstName`, `lastName`
- `email` (unique)
- `password` (hashed)
- `role` (`parent` | `employee` | `admin`)
- optional: phone/address/profile fields

Relationships:
- Parent users link to children through `Child.parent`
- Employees link to classroom through assignment logic (depends on your implementation)
- Notifications are linked to any user through `Notification.user`

Recommended indexes:
- `email` unique index

---

## 2) Child Collection (`Child`)

Purpose: store child profiles and their parent/classroom relationship.

Typical fields:
- identity: `firstName`, `lastName`, `dateOfBirth`
- `parent` → ObjectId reference to `User` (role parent)
- optional: `classroom` → ObjectId reference (if you have Classroom model or field)
- optional medical details, emergency contact

Relationships:
- One parent has many children
- A child can have many attendance records, payments, notes, alerts

Recommended indexes:
- `parent`
- `classroom` (if used often)

---

## 3) Attendance Collection (`Attendance`)

Purpose: record daily presence.

Typical fields:
- `child` → ref Child
- `date`
- `status` (`present`/`absent`)
- optional: `checkInTime`, `checkOutTime`
- optional: `recordedBy` (employee user)

Recommended indexes:
- `{ child: 1, date: 1 }` for fast per-child daily lookups

---

## 4) Activity Collection (`Activity`)

Purpose: record classroom/child activities.

Typical fields:
- `title`, `description`
- `date`
- `createdBy` → employee user
- optional: `classroom`
- optional: `children` array of child IDs

Used for:
- Daily reports
- Parent viewing
- Notifications

---

## 5) Payment Collection (`Payment`)

Purpose: invoices and payment tracking.

Typical fields:
- `child` → ref Child
- `parent` → ref User (parent)
- `amount`, `currency`
- `type` (registration, monthly, etc.)
- `status` (`pending` | `paid` | `overdue`)
- `dueDate`
- optional: invoice fields (invoice number, pdf link)

Recommended indexes:
- `{ parent: 1, status: 1 }`
- `{ child: 1, dueDate: -1 }`

---

## 6) Notification Collection (`Notification`)

Purpose: in-app notifications per user.

Fields:
- `user` → ref User (receiver)
- `type` (medical, payment, activity, system)
- `title`, `message`
- `priority` (low/medium/high)
- `isRead` boolean
- timestamps

Recommended indexes:
- `{ user: 1, createdAt: -1 }`
- `{ user: 1, isRead: 1 }`

---

## 7) MedicalAlert Collection (`MedicalAlert`)

Purpose: record health incidents/conditions per child, scoped to classroom.

Fields:
- `child` → ref Child (required)
- `classroom` → ref Classroom ID (required if filtering by classroom)
- `type` enum: `allergy | medication | condition | incident`
- `severity` enum: `low | medium | high | critical`
- `description` (required)
- optional: `instructions`, `medication`, `startDate`, `endDate`
- `reportedBy` → ref User (employee)
- `reportedDate`
- `isResolved`, `resolvedDate`, `resolvedBy`

Recommended indexes:
- `{ classroom: 1, reportedDate: -1 }`
- `{ child: 1, isResolved: 1 }`

---

## 8) Relationship Summary

```text
User(parent) 1 ---- N Child
Child 1 ---- N Attendance
Child 1 ---- N Payment
Child 1 ---- N MedicalAlert
User(any role) 1 ---- N Notification
User(employee) 1 ---- N Activity (createdBy)