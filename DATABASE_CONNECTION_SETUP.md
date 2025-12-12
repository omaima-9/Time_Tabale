# Database Connection Setup Guide

This guide will help you connect the backend to your PostgreSQL database.

## Step 1: Create Environment File

Since `.env` files are protected, you need to manually create the environment configuration file.

### Create `backend/.env` file:

```env
# Server Configuration
PORT=3001
NODE_ENV=development

# Database Configuration
DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=postgres
DB_DATABASE=timetabling_db

# JWT Configuration
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
JWT_EXPIRES_IN=7d

# CORS Configuration
CORS_ORIGIN=http://localhost:5173

# Solver Service Configuration
SOLVER_SERVICE_URL=http://localhost:5000

# File Upload Configuration
MAX_FILE_SIZE=10485760
```

**Important:** 
- Replace `DB_PASSWORD=postgres` with your actual PostgreSQL password
- Change `JWT_SECRET` to a secure random string in production

## Step 2: Create the Database

### Option A: Using the SQL Script

Run the initialization script:

```powershell
# Connect to PostgreSQL
psql -U postgres

# Then run the script
\i db/init_db.sql
```

Or manually:

```powershell
psql -U postgres -c "CREATE DATABASE timetabling_db;"
```

### Option B: Using pgAdmin

1. Open pgAdmin
2. Connect to your PostgreSQL server
3. Right-click on "Databases" → Create → Database
4. Name: `timetabling_db`
5. Click Save

### Option C: Using Docker

```powershell
docker run --name timetabling-postgres `
  -e POSTGRES_PASSWORD=postgres `
  -e POSTGRES_DB=timetabling_db `
  -p 5432:5432 `
  -d postgres:14
```

## Step 3: Verify Database Connection

Start the backend server:

```powershell
cd backend
npm run dev
```

You should see:
```
Database connected successfully
Server running on port 3001
```

If you see connection errors:

1. **"password authentication failed"**
   - Check your password in `backend/.env`
   - Verify PostgreSQL password is correct

2. **"database does not exist"**
   - Create the database (see Step 2)

3. **"connection refused"**
   - PostgreSQL service is not running
   - Check port 5432 is not blocked
   - Verify DB_HOST is correct

## Step 4: Import CSV Data (Optional)

If you want to import the CSV data from the `db/csv` folder:

```powershell
cd backend
npm run import:csv
```

This will:
- Import institutions, departments, users, courses, groups, and halls
- Create default passwords for users: `Password123!`
- Skip existing records (won't duplicate)

**Note:** Make sure the database tables are created first (they will be auto-created when you start the backend in development mode).

## Step 5: Create Initial Admin User

Create a super admin user:

```powershell
cd backend
npm run seed:admin
```

This creates:
- Email: `admin@timetabling.com`
- Password: `Admin123!`

**Important:** Change the password after first login!

## Verification Checklist

- [ ] `backend/.env` file exists with correct database credentials
- [ ] PostgreSQL is running
- [ ] Database `timetabling_db` exists
- [ ] Backend starts without connection errors
- [ ] Tables are created automatically (check with `\dt` in psql)
- [ ] CSV data imported (optional)
- [ ] Admin user created (optional)

## Testing the Connection

You can test the connection by:

1. Starting the backend: `cd backend && npm run dev`
2. Checking the health endpoint: `http://localhost:3001/api/health`
3. Verifying database tables exist:
   ```powershell
   psql -U postgres -d timetabling_db -c "\dt"
   ```

## Troubleshooting

### PostgreSQL Not Running

```powershell
# Check service status
Get-Service -Name postgresql*

# Start service
Start-Service postgresql-X.X
```

### Port Already in Use

If port 5432 is already in use:
- Change `DB_PORT` in `backend/.env` to a different port
- Or stop the conflicting PostgreSQL instance

### Permission Denied

- Make sure you're using the correct username (usually `postgres`)
- Verify password is correct
- Check PostgreSQL authentication settings in `pg_hba.conf`

## Next Steps

Once the database is connected:

1. Start the backend: `cd backend && npm run dev`
2. Start the frontend: `cd frontend && npm run dev`
3. Login with admin credentials
4. Start using the application!

