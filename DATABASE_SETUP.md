# Database Setup Guide

## Step 1: Install PostgreSQL (if not installed)

### Option A: Download and Install

1. Download PostgreSQL from: https://www.postgresql.org/download/windows/
2. Run the installer
3. During installation, remember:
   - **Port**: 5432 (default)
   - **Superuser password**: Choose a strong password (remember this!)
   - **Data directory**: Default is fine

### Option B: Using Chocolatey (if you have it)

```powershell
choco install postgresql
```

### Option C: Using Docker (Recommended for Development)

```powershell
docker run --name timetabling-postgres -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=timetabling_db -p 5432:5432 -d postgres:14
```

## Step 2: Verify PostgreSQL is Running

### Check if PostgreSQL service is running:

```powershell
Get-Service -Name postgresql*
```

### Start PostgreSQL service (if stopped):

```powershell
# Find the service name first
Get-Service -Name postgresql*

# Then start it (replace X.X with your version)
Start-Service postgresql-X.X
```

### Or use Services GUI:

1. Press `Win + R`, type `services.msc`
2. Find "postgresql" service
3. Right-click → Start

## Step 3: Create the Database

### Method 1: Using pgAdmin (GUI - Easiest)

1. Open **pgAdmin** (installed with PostgreSQL)
2. Connect to your PostgreSQL server
3. Right-click on "Databases" → Create → Database
4. Name: `timetabling_db`
5. Click Save

### Method 2: Using Command Line (psql)

```powershell
# Add PostgreSQL bin to PATH (if not already)
# Usually: C:\Program Files\PostgreSQL\14\bin

# Connect to PostgreSQL
psql -U postgres

# Enter your password when prompted
# Then run:
CREATE DATABASE timetabling_db;

# Exit
\q
```

### Method 3: Using SQL Command Directly

```powershell
# If psql is in PATH
psql -U postgres -c "CREATE DATABASE timetabling_db;"
```

### Method 4: Using Docker (if using Docker)

The database is already created! Skip to Step 4.

## Step 4: Configure Backend Environment

Create `backend/.env` file:

```env
PORT=3001
NODE_ENV=development

# Database Configuration
DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=your_postgres_password_here
DB_DATABASE=timetabling_db

# JWT Configuration
JWT_SECRET=your-super-secret-jwt-key-change-in-production
JWT_EXPIRES_IN=7d

# CORS
CORS_ORIGIN=http://localhost:5173

# Solver Service
SOLVER_SERVICE_URL=http://localhost:5000

# File Upload
MAX_FILE_SIZE=10485760
```

**Important:** Replace `your_postgres_password_here` with your actual PostgreSQL password!

## Step 5: Test Database Connection

### Test from Backend:

```powershell
cd backend
npm run dev
```

You should see:

```
Database connected successfully
Server running on port 3001
```

### If you see connection errors:

**Error: "password authentication failed"**

- Check your password in `backend/.env`
- Verify PostgreSQL password is correct

**Error: "database does not exist"**

- Create the database (see Step 3)

**Error: "connection refused"**

- PostgreSQL service is not running
- Check port 5432 is not blocked
- Verify DB_HOST is correct

## Step 6: Verify Tables are Created

Once backend starts successfully, TypeORM will automatically create tables (in development mode).

You can verify by connecting to the database:

```powershell
psql -U postgres -d timetabling_db

# List tables
\dt

# Should show tables like: users, institutions, courses, etc.
```

## Troubleshooting

### PostgreSQL Not in PATH

Add PostgreSQL bin directory to your PATH:

1. Find PostgreSQL installation (usually `C:\Program Files\PostgreSQL\14\bin`)
2. Add to System Environment Variables → PATH

### Can't Connect to Database

1. Check PostgreSQL is running: `Get-Service postgresql*`
2. Check port 5432 is open: `netstat -an | findstr 5432`
3. Verify credentials in `backend/.env`

### Permission Denied

- Make sure you're using the correct username (usually `postgres`)
- Verify password is correct
- Check PostgreSQL authentication settings in `pg_hba.conf`

### Using Different Database

If you want to use SQLite for development (simpler, no setup):

1. Install: `npm install sqlite3`
2. Change `backend/src/config/database.ts`:
   ```typescript
   type: 'sqlite',
   database: 'timetabling.db',
   ```

## Quick Start (Docker)

If you have Docker, this is the fastest way:

```powershell
# Start PostgreSQL container
docker run --name timetabling-postgres `
  -e POSTGRES_PASSWORD=postgres `
  -e POSTGRES_DB=timetabling_db `
  -p 5432:5432 `
  -d postgres:14

# Your backend/.env should have:
# DB_PASSWORD=postgres
# DB_DATABASE=timetabling_db
```

## Next Steps

Once database is set up:

1. Start backend: `cd backend && npm run dev`
2. Tables will be created automatically
3. Create your first user via registration page
4. Start using the application!
