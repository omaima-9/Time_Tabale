# Troubleshooting Login Issues

## Quick Fix: Create First User via Registration Page

Since the seed script has issues, use the registration page instead:

### Steps:

1. **Make sure backend is running:**
   ```bash
   cd backend
   npm run dev
   ```

2. **Make sure frontend is running:**
   ```bash
   cd frontend
   npm run dev
   ```

3. **Open browser:** `http://localhost:5173`

4. **On the login page**, you should see a link: **"Create the first Super Admin account"**

5. **Click that link** and fill in the registration form:
   - Name: Your name
   - Email: Your email (e.g., `admin@example.com`)
   - Password: Choose a password (min 8 characters)
   - Role: Will be automatically set to "Super Admin" (disabled field)
   - Institution ID: Leave empty
   - Department ID: Leave empty

6. **Submit the form** - this will create your first Super Admin account

7. **You'll be redirected to login page** - use the credentials you just created

## Common Login Issues

### "Invalid credentials"
- **Check:** Email and password are correct
- **Check:** User exists in database
- **Check:** User is active (`isActive = true`)

### "Login failed" (generic error)
- **Check:** Backend server is running on port 3001
- **Check:** Database connection is working
- **Check:** Check backend console for error messages

### No users exist
- Use the registration page to create the first user
- Or check if database has any users:
  ```sql
  SELECT email, role FROM users;
  ```

## Verify User Exists

If you want to check if a user exists in the database:

```sql
-- Connect to PostgreSQL
psql timetabling_db

-- Check users
SELECT id, name, email, role, "isActive" FROM users;
```

## Manual User Creation (SQL)

If registration page doesn't work, you can create a user manually:

```sql
-- Generate password hash first (use Node.js or online bcrypt tool)
-- For password "Admin123!", the hash would be something like:
-- $2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy

INSERT INTO users (id, name, email, "passwordHash", role, "isActive", "createdAt", "updatedAt")
VALUES (
  gen_random_uuid(),
  'Super Admin',
  'admin@example.com',
  '$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy', -- Replace with actual hash
  'super_admin',
  true,
  NOW(),
  NOW()
);
```

**To generate password hash:**
```javascript
const bcrypt = require('bcryptjs');
bcrypt.hash('Admin123!', 10).then(console.log);
```

## Still Having Issues?

1. Check backend console for detailed error messages
2. Check browser console (F12) for frontend errors
3. Verify database connection in `backend/.env`
4. Make sure all services are running (backend, frontend, database)

