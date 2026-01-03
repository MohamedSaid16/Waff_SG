 
# Employee Guide

## Employee Dashboard
Route: `/employee/dashboard`

Shows:
- Assigned classroom information
- Children count
- Quick access to attendance, activities, notes, and medical alerts

## Attendance
Route: `/employee/attendance`

Use this page to:
- Mark children present/absent
- Record check-in/check-out (if implemented)

## Daily Activities
Route: `/employee/activities`

Use this page to:
- Add an activity (title, description, date)
- Optionally associate the activity with children/classroom

Parents may receive notifications about new activities.

## Child Notes
Route: `/employee/child-notes`

Use this page to:
- Add notes about children (behavior, learning, health, general)
- Keep daily tracking organized

Parents may receive notifications for important notes.

## Medical Alerts
Route: `/employee/medical-alerts`

Use this page to:
- Create new medical alerts:
  - type: allergy / medication / condition / incident
  - severity: low / medium / high / critical
  - description and instructions
- Resolve existing alerts once handled

When you create a medical alert:
- Parent receives a notification
- Admins receive a notification

## Notifications
Route: `/notifications`

Employees may receive notifications based on configuration (optional).