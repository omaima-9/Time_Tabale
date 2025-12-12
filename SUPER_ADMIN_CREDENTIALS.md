# Super Admin Credentials

## Default Super Admin Account

After running the seed script, you can use these credentials:

**Email:** `admin@timetabling.com`  
**Password:** `Admin123!`

## How to Create the Super Admin

### Option 1: Using Seed Script (Recommended)

```bash
cd backend
npm run seed:admin
```

This will create a Super Admin user with the credentials above.

### Option 2: Using Registration Page

1. Start the backend and frontend servers
2. Go to `http://localhost:5173`
3. Click "Create the first Super Admin account" link on the login page
4. Fill in the registration form
5. The first user will automatically be created as Super Admin

### Option 3: Manual Database Insert

If you prefer to create it manually via SQL:

```sql
-- Connect to PostgreSQL
psql timetabling_db

-- Generate password hash for 'Admin123!' using bcrypt
-- You can use an online bcrypt generator or Node.js:
-- const bcrypt = require('bcryptjs');
-- bcrypt.hash('Admin123!', 10).then(console.log);

INSERT INTO users (id, name, email, "passwordHash", role, "isActive", "createdAt", "updatedAt")
VALUES (
  gen_random_uuid(),
  'Super Admin',
  'admin@timetabling.com',
  '$2a$10$YOUR_BCRYPT_HASH_HERE', -- Replace with actual bcrypt hash
  'super_admin',
  true,
  NOW(),
  NOW()
);
```

## Security Note

⚠️ **IMPORTANT:** Change the default password immediately after first login!

## After Login

Once logged in as Super Admin, you can:

1. Create institutions
2. Create other users (Admin, Scheduler, Teacher, Student)
3. Manage all system settings
4. Access all features
