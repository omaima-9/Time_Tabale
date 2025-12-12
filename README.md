# Automated Timetabling Web Application

A comprehensive role-based web application for generating conflict-free teaching schedules using Google OR-Tools CP-SAT constraint programming.

## Features

- **Multi-tenant Architecture**: Support for multiple institutions with isolated data
- **Role-Based Access Control**: Super Admin, Admin, Scheduler, Teacher, and Student roles
- **Automated Scheduling**: Google OR-Tools CP-SAT solver for optimal schedule generation
- **PDF Export**: Printable schedules for Master, Groups, Teachers, and Halls
- **CSV Import/Export**: Bulk data management for all entities
- **Availability Management**: Flexible availability settings for teachers, groups, and halls
- **Audit Logging**: Complete audit trail of all system changes

## Tech Stack

### Backend
- Node.js + Express + TypeScript
- TypeORM + PostgreSQL
- JWT Authentication
- Google OR-Tools (Python service)
- PDFKit for PDF generation

### Frontend
- React + TypeScript
- Vite
- React Router
- Material-UI or Tailwind CSS

## Project Structure

```
.
├── backend/          # Express API server
├── frontend/         # React application
├── solver-service/   # Python OR-Tools service
└── README.md
```

## Getting Started

### Prerequisites
- Node.js 18+
- PostgreSQL 14+
- Python 3.9+ (for solver service)

### Installation

1. Install all dependencies:
```bash
npm run install:all
```

2. Set up environment variables:
- Copy `backend/.env.example` to `backend/.env` and configure
- Copy `frontend/.env.example` to `frontend/.env` and configure

3. Set up database:
```bash
cd backend
npm run migration:run
```

4. Start development servers:
```bash
npm run dev
```

## User Roles

- **Super Admin**: Full system access, can create institutions and manage admins
- **Admin**: Manage users/roles, CRUD on all entities, run/approve schedules
- **Scheduler**: Edit inputs, run solver, cannot manage users
- **Teacher**: View own schedule, set availability/preferences
- **Student**: Read-only access to published schedules

## API Documentation

See `backend/README.md` for detailed API documentation.

## License

ISC

