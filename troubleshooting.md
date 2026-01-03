 
# Troubleshooting

## 1) Frontend opens but data does not load

### Symptoms
- Empty tables/lists
- “No data found” even though database has data
- Console shows 404/500 errors

### Fix checklist
- Ensure backend is running
- Ensure `REACT_APP_API_URL` is correct in `frontend/.env`
- Restart frontend after changing `.env`
- Check backend logs for errors

---

## 2) 401 Unauthorized

### Causes
- Missing token
- Token expired
- Axios not sending Authorization header

### Fix
- Login again
- Confirm token storage in AuthContext
- Confirm Axios interceptor is adding:
  `Authorization: Bearer <token>`

---

## 3) 403 Forbidden

### Cause
- You are logged in with a role that cannot access the route

### Fix
- Login with correct role (admin vs employee vs parent)
- Verify `ProtectedRoute` settings and backend `roleMiddleware`

---

## 4) MongoDB connection errors

### Symptoms
- Backend fails to start
- Logs show “MongoNetworkError”, “ECONNREFUSED”, etc.

### Fix
- Verify `backend/.env` has correct `MONGO_URI`
- If local MongoDB:
  - Ensure MongoDB service is running
- If MongoDB Atlas:
  - Whitelist your IP
  - Ensure username/password are correct

---

## 5) Medical alerts not appearing for employee

### Common cause
- Alerts are filtered by classroom but MedicalAlert schema does not include `classroom`

### Fix
- Ensure `MedicalAlert` model has `classroom` field
- Ensure creation controller sets `classroom`
- Create a new alert after updating schema

---

## 6) CORS errors

### Symptoms
Browser console:
- “blocked by CORS policy”

### Fix
- In backend, enable CORS for frontend origin:
  - `http://localhost:3000`
- Restart backend

---

## 7) Still not working?

Collect:
- Browser console errors
- Network tab: failing request URL + status code + response
- Backend terminal logs

Then debug the specific endpoint/controller.