# Quick Start Guide

## Prerequisites Check
- ✅ Node.js 18+ installed
- ✅ PostgreSQL 14+ installed and running
- ✅ Python 3.9+ installed (for solver)

## Step-by-Step Run Instructions

### 1. Install Dependencies

```bash
# From root directory
npm run install:all
```

Or install individually:
```bash
npm install                    # Root workspace
cd backend && npm install     # Backend
cd ../frontend && npm install # Frontend
```

### 2. Set Up Database

Create PostgreSQL database:
```bash
createdb timetabling_db
```

### 3. Configure Backend Environment

Create `backend/.env` file:
```env
PORT=3001
NODE_ENV=development

DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=postgres
DB_DATABASE=timetabling_db

JWT_SECRET=your-super-secret-jwt-key-change-in-production
JWT_EXPIRES_IN=7d

CORS_ORIGIN=http://localhost:5173

SOLVER_SERVICE_URL=http://localhost:5000

MAX_FILE_SIZE=10485760
```

**Update the database credentials** to match your PostgreSQL setup.

### 4. Start All Services

You need **3 terminal windows**:

#### Terminal 1: Solver Service
```bash
cd solver-service
pip install -r requirements.txt
python app.py
```
✅ Should show: `Running on http://0.0.0.0:5000`

#### Terminal 2: Backend API
```bash
cd backend
npm run dev
```
✅ Should show: `Server running on port 3001`

#### Terminal 3: Frontend
```bash
cd frontend
npm run dev
```
✅ Should show: `Local: http://localhost:5173`

### 5. Access Application

Open browser: **http://localhost:5173**

### 6. Create First User

Since there's no user yet, you need to create a Super Admin. Options:

**Option A: Using SQL (Recommended)**
```sql
-- Connect to PostgreSQL
psql timetabling_db

-- Insert super admin (password: admin123)
INSERT INTO users (id, name, email, "passwordHash", role, "isActive", "createdAt", "updatedAt")
VALUES (
  gen_random_uuid(),
  'Super Admin',
  'admin@example.com',
  '$2a$10$rOzJqZqKqKqKqKqKqKqKqOqKqKqKqKqKqKqKqKqKqKqKqKqKqKqKqKqKqKqKq',
  'super_admin',
  true,
  NOW(),
  NOW()
);
```

**Option B: Using Node.js script**
Create a temporary script to hash password and insert user.

**Default credentials after SQL insert:**
- Email: `admin@example.com`
- Password: `admin123` (if you used the hash above)

## Alternative: Run Backend + Frontend Together

From root directory:
```bash
npm run dev
```

This runs both backend and frontend concurrently (solver still needs separate terminal).

## Verify Everything is Running

1. **Solver**: http://localhost:5000/health → Should return `{"status":"ok"}`
2. **Backend**: http://localhost:3001/api/health → Should return `{"status":"ok","timestamp":"..."}`
3. **Frontend**: http://localhost:5173 → Should show login page

## Troubleshooting

### "Cannot connect to database"
- Check PostgreSQL is running: `pg_isready`
- Verify credentials in `backend/.env`
- Ensure database exists: `psql -l | grep timetabling_db`

### "Solver service not responding"
- Check Python version: `python --version` (needs 3.9+)
- Install OR-Tools: `pip install ortools`
- Check port 5000 is free

### "Port already in use"
- Backend (3001): Change `PORT` in `backend/.env`
- Frontend (5173): Change in `frontend/vite.config.ts`
- Solver (5000): Change in `solver-service/app.py`

## Next Steps After Running

1. Login with super admin account
2. Create an institution
3. Add users, courses, groups, halls
4. Create a term and timeslots
5. Generate your first schedule!

