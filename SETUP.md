# Setup Guide

Complete setup instructions for the Automated Timetabling Web Application.

## Prerequisites

- Node.js 18+ and npm
- PostgreSQL 14+
- Python 3.9+ (for solver service)

## Step 1: Install Dependencies

From the root directory:

```bash
npm run install:all
```

This will install dependencies for:
- Root workspace
- Backend
- Frontend

## Step 2: Database Setup

1. Create PostgreSQL database:
```bash
createdb timetabling_db
```

2. Configure backend environment:
```bash
cd backend
cp .env.example .env
# Edit .env with your database credentials
```

Update `backend/.env`:
```
DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=your_password
DB_DATABASE=timetabling_db
JWT_SECRET=your-secret-key-here
```

## Step 3: Start Solver Service

In a separate terminal:

```bash
cd solver-service
pip install -r requirements.txt
python app.py
```

The solver service should start on `http://localhost:5000`.

## Step 4: Start Backend

In a new terminal:

```bash
cd backend
npm run dev
```

The backend API will start on `http://localhost:3001`.

## Step 5: Start Frontend

In a new terminal:

```bash
cd frontend
npm run dev
```

The frontend will start on `http://localhost:5173`.

## Step 6: Create First User

The first user must be created as a Super Admin. You can do this via:

1. Direct database insert, or
2. Using a migration/seed script

Example SQL (use a secure password hash):
```sql
INSERT INTO users (id, name, email, "passwordHash", role, "createdAt", "updatedAt")
VALUES (
  gen_random_uuid(),
  'Super Admin',
  'admin@example.com',
  '$2a$10$...', -- Use bcrypt to hash your password
  'super_admin',
  NOW(),
  NOW()
);
```

Or use the registration endpoint after manually creating the first super admin.

## Step 7: Access the Application

1. Open `http://localhost:5173` in your browser
2. Login with your super admin credentials
3. Create an institution
4. Start adding courses, groups, halls, etc.

## Development Workflow

### Running All Services

From root:
```bash
npm run dev
```

This uses `concurrently` to run both backend and frontend.

### Database Migrations

```bash
cd backend
npm run migration:generate -- -n MigrationName
npm run migration:run
```

### Building for Production

```bash
npm run build
```

This builds both backend and frontend.

## Troubleshooting

### Database Connection Issues
- Verify PostgreSQL is running
- Check credentials in `backend/.env`
- Ensure database exists

### Solver Service Not Responding
- Check Python version (3.9+)
- Verify OR-Tools is installed: `pip list | grep ortools`
- Check solver service logs

### Port Already in Use
- Backend: Change `PORT` in `backend/.env`
- Frontend: Change port in `frontend/vite.config.ts`
- Solver: Change port in `solver-service/app.py`

## Next Steps

1. Create your first institution
2. Add departments
3. Import data via CSV (see CSV templates in documentation)
4. Create terms and timeslots
5. Set up availability
6. Generate your first schedule!

