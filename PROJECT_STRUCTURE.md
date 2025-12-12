# Project Structure & Outline

## Automated Timetabling Web Application

A comprehensive role-based web application for generating conflict-free teaching schedules using Google OR-Tools CP-SAT constraint programming.

---

## Folder Hierarchy

```
timetabling-app/
│
├── backend/                          # Express API Server (TypeScript)
│   ├── src/
│   │   ├── config/
│   │   │   └── database.ts          # Database configuration (TypeORM)
│   │   │
│   │   ├── entities/                # TypeORM Entity Definitions
│   │   │   ├── Assignment.ts        # Course-to-timeslot assignments
│   │   │   ├── AuditLog.ts          # System audit trail
│   │   │   ├── Availability.ts      # Teacher/Group/Hall availability
│   │   │   ├── Course.ts            # Course definitions
│   │   │   ├── Department.ts         # Department/organizational units
│   │   │   ├── Enrollment.ts        # Group-course enrollments
│   │   │   ├── Group.ts             # Student groups
│   │   │   ├── Hall.ts              # Classroom/venue definitions
│   │   │   ├── Institution.ts       # Multi-tenant institutions
│   │   │   ├── PrintTemplate.ts     # PDF template configurations
│   │   │   ├── Schedule.ts           # Generated schedule instances
│   │   │   ├── Term.ts               # Academic terms/semesters
│   │   │   ├── Timeslot.ts          # Time slot definitions
│   │   │   ├── User.ts               # User accounts and authentication
│   │   │   └── WeekPattern.ts       # Weekly pattern configurations
│   │   │
│   │   ├── middleware/              # Express Middleware
│   │   │   ├── auditLogger.ts       # Audit logging middleware
│   │   │   ├── auth.ts              # JWT authentication & authorization
│   │   │   └── errorHandler.ts      # Global error handling
│   │   │
│   │   ├── routes/                  # API Route Handlers
│   │   │   ├── auth.routes.ts       # Authentication endpoints
│   │   │   ├── availability.routes.ts    # Availability management
│   │   │   ├── course.routes.ts     # Course CRUD operations
│   │   │   ├── enrollment.routes.ts # Enrollment management
│   │   │   ├── group.routes.ts      # Group management
│   │   │   ├── hall.routes.ts       # Hall/venue management
│   │   │   ├── import-export.routes.ts   # CSV import/export
│   │   │   ├── institution.routes.ts     # Institution management
│   │   │   ├── schedule.routes.ts   # Schedule generation & management
│   │   │   ├── term.routes.ts       # Term management
│   │   │   ├── timeslot.routes.ts   # Timeslot management
│   │   │   └── user.routes.ts       # User management
│   │   │
│   │   ├── services/                # Business Logic Services
│   │   │   ├── pdf.service.ts       # PDF generation (PDFKit)
│   │   │   └── solver.service.ts    # Integration with Python solver
│   │   │
│   │   ├── scripts/                 # Utility Scripts
│   │   │   └── seed-admin.ts        # Database seeding (admin users)
│   │   │
│   │   └── index.ts                 # Express app entry point
│   │
│   ├── package.json                 # Backend dependencies
│   ├── tsconfig.json                # TypeScript configuration
│   └── README.md                    # Backend documentation
│
├── frontend/                        # React Application (TypeScript + Vite)
│   ├── src/
│   │   ├── contexts/                # React Context Providers
│   │   │   └── AuthContext.tsx      # Authentication state management
│   │   │
│   │   ├── pages/                   # Page Components
│   │   │   ├── Dashboard.tsx        # Main dashboard
│   │   │   ├── Login.tsx            # Login page
│   │   │   └── Register.tsx         # Registration page
│   │   │
│   │   ├── types/                   # TypeScript Type Definitions
│   │   │   └── index.ts             # Shared type definitions
│   │   │
│   │   ├── App.tsx                  # Root React component
│   │   ├── main.tsx                 # Application entry point
│   │   └── index.css                # Global styles
│   │
│   ├── index.html                   # HTML template
│   ├── package.json                 # Frontend dependencies
│   ├── tsconfig.json                # TypeScript configuration
│   ├── tsconfig.node.json           # Node.js TypeScript config
│   └── vite.config.ts               # Vite build configuration
│
├── solver-service/                  # Python OR-Tools Service
│   ├── app.py                       # Flask/FastAPI application
│   ├── requirements.txt             # Python dependencies
│   └── README.md                    # Solver service documentation
│
├── trial/                           # Test/Example Module
│   ├── index.ts                     # Source TypeScript file
│   ├── index.js                     # Compiled JavaScript
│   ├── index.d.ts                   # Type definitions
│   ├── package.json                 # Package configuration
│   └── tsconfig.json                # TypeScript configuration
│
├── node_modules/                    # Root workspace dependencies
│
├── package.json                     # Root workspace configuration
├── package-lock.json                # Dependency lock file
│
├── README.md                        # Main project documentation
├── SETUP.md                         # Setup instructions
├── QUICKSTART.md                    # Quick start guide
├── DATABASE_SETUP.md                # Database setup guide
├── SUPER_ADMIN_CREDENTIALS.md       # Admin credentials documentation
└── TROUBLESHOOTING_LOGIN.md         # Login troubleshooting guide
```

---

## Project Outline

### 1. **Backend** (`/backend`)
   - **Purpose**: RESTful API server providing all business logic and data management
   - **Technology**: Node.js, Express, TypeScript, TypeORM, PostgreSQL
   - **Key Components**:
     - **Entities**: Database schema definitions using TypeORM decorators
     - **Routes**: API endpoints organized by resource (auth, courses, schedules, etc.)
     - **Services**: Business logic for PDF generation and solver integration
     - **Middleware**: Authentication, authorization, error handling, and audit logging
     - **Config**: Database connection and environment configuration

### 2. **Frontend** (`/frontend`)
   - **Purpose**: User interface for the timetabling application
   - **Technology**: React, TypeScript, Vite, Material-UI
   - **Key Components**:
     - **Pages**: Main application views (Dashboard, Login, Register)
     - **Contexts**: Global state management (authentication)
     - **Types**: Shared TypeScript interfaces and types
     - **App**: Root component with routing configuration

### 3. **Solver Service** (`/solver-service`)
   - **Purpose**: Standalone Python microservice for schedule optimization
   - **Technology**: Python, Google OR-Tools CP-SAT
   - **Functionality**: Receives scheduling constraints and returns optimized schedule solutions

### 4. **Trial** (`/trial`)
   - **Purpose**: Test/example module (possibly for development or experimentation)
   - **Status**: Contains compiled TypeScript output

### 5. **Root Configuration**
   - **Workspace Management**: Monorepo setup using npm workspaces
   - **Scripts**: Unified commands for development, building, and deployment
   - **Documentation**: Setup guides, troubleshooting, and feature documentation

---

## Key Features by Component

### Backend Features
- ✅ Multi-tenant architecture (Institution isolation)
- ✅ Role-based access control (Super Admin, Admin, Scheduler, Teacher, Student)
- ✅ JWT authentication
- ✅ CSV import/export for bulk operations
- ✅ PDF generation for schedules
- ✅ Audit logging for all changes
- ✅ Integration with Python solver service

### Frontend Features
- ✅ React-based single-page application
- ✅ Material-UI components
- ✅ Authentication context management
- ✅ Responsive design

### Solver Service Features
- ✅ Constraint programming using Google OR-Tools
- ✅ Conflict-free schedule generation
- ✅ Optimization based on preferences and constraints

---

## Data Flow

1. **User Authentication**: Frontend → Backend (JWT) → Database
2. **Schedule Generation**: Frontend → Backend → Solver Service → Backend → Database
3. **PDF Export**: Frontend → Backend → PDF Service → Download
4. **CSV Import**: Frontend → Backend → Parser → Database
5. **Audit Logging**: All Backend operations → AuditLog entity

---

## Development Workflow

1. **Installation**: `npm run install:all`
2. **Database Setup**: Configure PostgreSQL and run migrations
3. **Development**: `npm run dev` (runs both backend and frontend)
4. **Solver Service**: Run separately (Python service)
5. **Building**: `npm run build` (compiles both backend and frontend)

---

## Role Hierarchy

- **Super Admin**: Full system access, institution management
- **Admin**: Institution-level management, user/role management
- **Scheduler**: Schedule generation, input editing
- **Teacher**: View schedules, set availability
- **Student**: Read-only schedule access

---

## Technology Stack Summary

| Component | Technologies |
|-----------|-------------|
| **Backend** | Node.js, Express, TypeScript, TypeORM, PostgreSQL, JWT, PDFKit |
| **Frontend** | React, TypeScript, Vite, Material-UI, React Router |
| **Solver** | Python, Google OR-Tools CP-SAT |
| **Database** | PostgreSQL 14+ |
| **Authentication** | JWT (JSON Web Tokens) |
| **File Processing** | CSV (parse/stringify), PDF (PDFKit) |

