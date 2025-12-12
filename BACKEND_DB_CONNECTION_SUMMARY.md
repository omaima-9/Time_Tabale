# Backend Database Connection - Setup Summary

## ✅ What Has Been Set Up

### 1. Database Initialization Script
- **File**: `db/init_db.sql`
- **Purpose**: Creates the PostgreSQL database if it doesn't exist
- **Usage**: Run with `psql -U postgres -f db/init_db.sql`

### 2. Environment Configuration
- **File**: `backend/.env.example` (template)
- **Note**: You need to manually create `backend/.env` with your database credentials
- **See**: `DATABASE_CONNECTION_SETUP.md` for detailed instructions

### 3. CSV Data Import Script
- **File**: `backend/src/scripts/import-csv-data.ts`
- **Purpose**: Imports all CSV data from `db/csv/` folder into the database
- **Usage**: `npm run import:csv` (from backend directory)
- **Features**:
  - Imports institutions, departments, users, courses, groups, halls
  - Handles data type conversions
  - Skips existing records (no duplicates)
  - Sets default password `Password123!` for imported users

### 4. Database Configuration
- **File**: `backend/src/config/database.ts`
- **Status**: Already configured correctly
- **Features**:
  - Uses TypeORM DataSource
  - Auto-synchronizes schema in development mode
  - Connects to PostgreSQL using environment variables

### 5. Backend Entry Point
- **File**: `backend/src/index.ts`
- **Status**: Already configured to initialize database connection on startup
- **Behavior**: 
  - Connects to database on server start
  - Creates tables automatically in development mode
  - Exits with error if connection fails

## 📋 Quick Start Checklist

1. **Create `backend/.env` file** (copy from template in `DATABASE_CONNECTION_SETUP.md`)
   - Set `DB_PASSWORD` to your PostgreSQL password
   - Set `JWT_SECRET` to a secure random string

2. **Create the database**:
   ```powershell
   psql -U postgres -c "CREATE DATABASE timetabling_db;"
   ```

3. **Start the backend**:
   ```powershell
   cd backend
   npm run dev
   ```
   - This will automatically create all tables
   - You should see: "Database connected successfully"

4. **Import CSV data** (optional):
   ```powershell
   cd backend
   npm run import:csv
   ```

5. **Create admin user** (optional):
   ```powershell
   cd backend
   npm run seed:admin
   ```

## 🔧 Available Scripts

From `backend/` directory:

- `npm run dev` - Start development server (auto-creates tables)
- `npm run import:csv` - Import CSV data from `db/csv/` folder
- `npm run seed:admin` - Create default super admin user
- `npm run migration:generate` - Generate new migration
- `npm run migration:run` - Run pending migrations
- `npm run migration:revert` - Revert last migration

## 📁 File Structure

```
backend/
├── .env                    # ⚠️ Create this manually (see DATABASE_CONNECTION_SETUP.md)
├── .env.example            # Template (if created)
├── src/
│   ├── config/
│   │   └── database.ts     # ✅ Database configuration
│   ├── scripts/
│   │   ├── seed-admin.ts   # ✅ Create admin user
│   │   └── import-csv-data.ts  # ✅ Import CSV data
│   └── index.ts            # ✅ Server entry point (connects to DB)
└── package.json            # ✅ Updated with import:csv script

db/
├── init_db.sql             # ✅ Database creation script
└── csv/                    # ✅ CSV data files
    ├── institutions.csv
    ├── departments.csv
    ├── users.csv
    ├── courses.csv
    ├── groups.csv
    ├── halls.csv
    └── ...
```

## 🔍 Verification

To verify the connection is working:

1. **Check backend logs**:
   ```
   Database connected successfully
   Server running on port 3001
   ```

2. **Test health endpoint**:
   ```powershell
   curl http://localhost:3001/api/health
   ```

3. **Check database tables**:
   ```powershell
   psql -U postgres -d timetabling_db -c "\dt"
   ```
   Should show tables: users, institutions, departments, courses, groups, halls, etc.

## ⚠️ Important Notes

1. **Environment File**: The `.env` file is protected and must be created manually. See `DATABASE_CONNECTION_SETUP.md` for the exact content.

2. **Development Mode**: In development (`NODE_ENV=development`), TypeORM will automatically:
   - Create tables based on entities
   - Synchronize schema changes
   - Enable SQL logging

3. **Production Mode**: In production, set `NODE_ENV=production` and:
   - Disable `synchronize` (use migrations instead)
   - Use proper migrations for schema changes
   - Set secure `JWT_SECRET`

4. **CSV Import**: The import script handles:
   - Data type conversions (strings to numbers, booleans)
   - JSON parsing for complex fields
   - Relationship mapping (institutions, departments)
   - Default password generation for users

## 🐛 Troubleshooting

If you encounter issues, see `DATABASE_CONNECTION_SETUP.md` for detailed troubleshooting steps.

Common issues:
- **Connection refused**: PostgreSQL not running
- **Password authentication failed**: Wrong password in `.env`
- **Database does not exist**: Create database first
- **Tables not created**: Check `synchronize` is enabled in development mode

## 📚 Related Documentation

- `DATABASE_CONNECTION_SETUP.md` - Detailed setup instructions
- `DATABASE_SETUP.md` - Original database setup guide
- `SETUP.md` - General project setup
- `README.md` - Project overview

