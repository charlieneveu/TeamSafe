# TeamSafe - Team Wellness & Security Management System

## Overview

TeamSafe is a comprehensive team wellness and security management application designed to monitor and ensure the safety of team members working in various professional settings. The system provides real-time location tracking, automated safety alerts, visit management, and emergency response capabilities.

The application is built as a full-stack web solution with a React frontend and Express.js backend, utilizing PostgreSQL for data persistence and Replit Auth for authentication. It features responsive design for both desktop and mobile devices, ensuring team members can stay connected and safe regardless of their location.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
The client-side application is built with React 18 and TypeScript, utilizing a modern component-based architecture. The UI is implemented using shadcn/ui components built on Radix UI primitives, providing a consistent and accessible design system. Styling is handled through Tailwind CSS with a comprehensive design token system for theming.

The application employs client-side routing using Wouter for lightweight navigation management. State management is handled through React Query (TanStack Query) for server state synchronization, providing efficient caching, background updates, and optimistic updates. Form management utilizes React Hook Form with Zod validation schemas for type-safe form handling.

The responsive design adapts between desktop and mobile layouts, with dedicated navigation components for each screen size. The mobile-first approach ensures optimal user experience across all device types.

### Backend Architecture
The server is built on Express.js with TypeScript, following a modular route-based architecture. The application implements a RESTful API design with clear separation of concerns between authentication, business logic, and data access layers.

Authentication is handled through Replit's OpenID Connect (OIDC) implementation using Passport.js strategies. Session management utilizes PostgreSQL-backed session storage with configurable TTL settings for security.

The storage layer implements an interface-based design pattern, currently using an in-memory implementation but structured to easily migrate to database-backed storage. This provides flexibility for different deployment scenarios while maintaining consistent API contracts.

### Database Design
The schema is defined using Drizzle ORM with PostgreSQL as the target database. The database structure includes:

- **Users table**: Stores team member and duty manager profiles with role-based access control, including work location status (on-site/remote)
- **Visits table**: Tracks individual visits with location, duration, and status information
- **Safety Status table**: Maintains real-time safety status for each team member
- **Alerts table**: Manages safety alerts with severity levels and resolution tracking
- **Sessions table**: Handles authentication session persistence

The schema utilizes UUIDs for primary keys and includes proper indexing for performance optimization. Timestamps are used throughout for audit trails and temporal queries.

### Work Location Feature
The system tracks each team member's current work location (on-site or remote) for better team coordination and visibility:

- **Location Tracking**: Users can set their work location to "on-site" or "remote" via toggle tabs in the dashboard
- **Team Visibility**: All team members can see each other's work locations in multiple views:
  - **Team Members List**: Comprehensive roster showing all team members with work location badges
  - **Active Team Status**: Filtered view showing only members requiring attention (visiting/emergency/overdue)
- **Persistence**: Work location preferences are stored in the database and persist across sessions
- **Visual Indicators**: On-site locations show with a blue badge and building icon, remote locations show with a purple badge and map pin icon
- **API Support**: The PATCH /api/profile endpoint supports partial updates to allow users to change only their work location without updating other profile fields

### Team Management
The application supports multi-team membership allowing users to be part of multiple teams and switch between them:

- **Team Memberships**: Users can view all their team memberships via the Team Management dialog
- **Team Switching**: Members of multiple teams can easily switch between teams from the "My Teams" tab
- **Team Discovery**: Users can search for existing teams, join with team codes, or create new teams
- **Active Team**: The currently selected team determines which data and members are visible in the dashboard
- **Role Management**: Each team membership has its own role (team member or duty manager)

### Dashboard Layout
The dashboard provides multiple views for team coordination:

- **Your Current Status**: Personal safety status with work location selector and quick action buttons
- **Duty Manager Contact**: Quick access to current duty manager with call functionality
- **Team Members**: Complete roster of all team members showing work locations, safety status, roles, and contact options
- **Active Team Status**: Focused view of members currently on visits or requiring immediate attention
- **Active Alerts**: Priority display of unresolved safety alerts requiring team response

### Authentication and Authorization
The system implements Replit Auth using OpenID Connect for secure authentication. User sessions are stored in PostgreSQL with automatic cleanup of expired sessions. Role-based access control differentiates between team members and duty managers, with appropriate permissions for each user type.

CSRF protection is implemented through secure session cookies with HTTP-only flags. The authentication middleware ensures API endpoints are properly protected while allowing graceful degradation for unauthorized requests.

### API Design
The REST API follows conventional HTTP methods and status codes:
- **GET /api/auth/user**: Retrieve current user profile
- **POST /api/visits**: Create new patient visits
- **GET /api/visits**: Retrieve user's visit history
- **PATCH /api/visits/:id/complete**: Mark visits as completed
- **GET /api/safety-status**: Get current safety status
- **POST /api/safety-status**: Update safety status
- **GET /api/alerts**: Retrieve alerts for current user
- **POST /api/alerts**: Create new safety alerts

Error handling is standardized across all endpoints with consistent error response formats and appropriate HTTP status codes.

## External Dependencies

### Core Framework Dependencies
- **React 18**: Frontend framework with TypeScript support
- **Express.js**: Backend web framework for Node.js
- **Drizzle ORM**: Type-safe SQL query builder and schema management
- **Vite**: Frontend build tool and development server

### UI and Styling
- **shadcn/ui**: Component library built on Radix UI primitives
- **Radix UI**: Accessible component primitives for React
- **Tailwind CSS**: Utility-first CSS framework
- **Lucide React**: Icon library for consistent iconography

### Authentication and Session Management
- **Replit Auth**: OpenID Connect authentication provider
- **Passport.js**: Authentication middleware with OIDC strategy
- **connect-pg-simple**: PostgreSQL session store for Express

### Database and Storage
- **PostgreSQL**: Primary database (via DATABASE_URL environment variable)
- **@neondatabase/serverless**: PostgreSQL driver for serverless environments

### Development and Build Tools
- **TypeScript**: Type safety across frontend and backend
- **Vite**: Development server with hot module replacement
- **esbuild**: Fast JavaScript bundler for production builds
- **tsx**: TypeScript execution for development server

### Form Handling and Validation
- **React Hook Form**: Performant form library with minimal re-renders
- **Zod**: Runtime type validation and schema definition
- **@hookform/resolvers**: Integration between React Hook Form and Zod

### State Management and Data Fetching
- **TanStack React Query**: Server state management with caching
- **wouter**: Lightweight client-side routing

The application is configured to run in both development and production environments, with environment-specific optimizations and security configurations.