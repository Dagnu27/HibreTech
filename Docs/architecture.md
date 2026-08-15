Your `docs/08-architecture.md` is already strong and consistent with the previous documents. I would keep the existing architecture and make the requested additions without expanding the MVP unnecessarily.

Below is the finalized version.

# `docs/08-architecture.md`

# HiberTech — System Architecture

## 1. Purpose and Goals

This document defines the HiberTech MVP architecture, including its main components, communication patterns, data flow, deployment structure, and security boundaries.

The architecture is designed to provide:

* A maintainable and modular web application.
* Clear separation between frontend, backend, business logic, and data access.
* Real-time chat, task updates, and notifications.
* Secure workspace and user data.
* Simple testing and deployment for a small development team.
* A foundation for future expansion into AI, analytics, integrations, and mobile applications.

---

## 2. Architecture Style

HiberTech v1.0 uses a **modular monolith architecture** with layered design.

The architecture consists of:

* Layered application architecture.
* Modular monolith backend.
* REST API for standard CRUD operations.
* Socket.io/WebSocket for real-time features.
* MongoDB for document-based data storage.
* Cloud object storage for uploaded files.

A modular monolith is preferred for Version 1 because it keeps deployment, development, testing, and debugging relatively simple while allowing individual features to remain separated into independent modules.

Microservices can be considered later if system scale or organizational requirements justify the additional complexity.

---

# 3. Architecture Principles

HiberTech follows these core architecture principles.

### 3.1 Separation of Concerns

Frontend, API, business logic, data access, real-time communication, and external services should have clearly defined responsibilities.

### 3.2 Security First

Authentication, authorization, validation, file security, and sensitive-data protection must be enforced primarily on the server.

### 3.3 Modular Design

Each major business capability should be organized as an independent feature module where practical.

### 3.4 Scalability

The MVP should remain simple while allowing future components such as Redis, background workers, search, and independent services to be introduced when required.

### 3.5 Maintainability

Code should be organized consistently, documented appropriately, and designed so that new developers can understand and modify individual modules without unnecessary impact on the rest of the system.

---

# 4. System Context

The HiberTech system interacts with users and several external services.

```text
                         ┌─────────────────┐
                         │   User Browser  │
                         └────────┬────────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │    HiberTech    │
                         │   Web Platform  │
                         └────────┬────────┘
                                  │
              ┌───────────────────┼───────────────────┐
              │                   │                   │
              ▼                   ▼                   ▼
       ┌────────────┐      ┌────────────┐      ┌─────────────┐
       │  MongoDB   │      │  Socket.io │      │   Cloud     │
       │  Database  │      │ Real-Time  │      │   Storage   │
       └────────────┘      └────────────┘      └─────────────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │  Email Service  │
                         └─────────────────┘
```

The browser communicates with HiberTech through HTTPS/REST and WebSocket connections.

HiberTech communicates with MongoDB, cloud storage, and the email provider through controlled backend services.

---

# 5. High-Level Architecture

```text
User Browser
     │
     ▼
Next.js Web Application
     │
     ├── HTTPS/REST ──► Express API
     │                       │
     │                       ▼
     │              Authentication
     │                       │
     │                       ▼
     │              Authorization
     │                       │
     │                       ▼
     │               Validation
     │                       │
     │                       ▼
     │              Business Services
     │                  │    │    │
     │                  │    │    └──► Email Service
     │                  │    │
     │                  │    └───────► Cloud Storage
     │                  │
     │                  └────────────► MongoDB
     │
     └── WebSocket ─────► Socket.io
                              │
                              ▼
                       Real-Time Services
```

---

# 6. Main System Components

| Component         | Technology                 | Main Responsibility                                                                   |
| ----------------- | -------------------------- | ------------------------------------------------------------------------------------- |
| Web Client        | Next.js, React, TypeScript | UI, navigation, dashboards, projects, tasks, chat, files, notifications               |
| API Server        | Node.js, Express           | HTTP requests, validation, authentication, authorization, business operations, errors |
| Authentication    | JWT, bcrypt/Argon2         | Registration, login, password hashing, tokens, password reset                         |
| Authorization     | RBAC + membership checks   | Workspace membership, roles, project access, protected operations                     |
| Real-Time Service | Socket.io                  | Chat, presence, live task updates, notifications                                      |
| Business Services | Feature-based modules      | Core application rules and coordination                                               |
| Data Layer        | MongoDB + Mongoose         | Users, workspaces, projects, tasks, messages, files, notifications                    |
| File Storage      | Cloudinary or AWS S3       | Profile images, chat images, documents, task attachments                              |
| Email Service     | Email provider             | Password resets, invitations, important account notifications                         |

---

# 7. Core Feature Modules

## Workspace

Responsible for:

* Workspace creation.
* Member invitations.
* Member removal.
* Workspace membership.
* Workspace roles.
* Workspace access control.

## Projects

Responsible for:

* Project creation.
* Project updates.
* Project archiving.
* Project status.
* Project membership.
* Project organization.

## Tasks

Responsible for:

* Task creation.
* Task assignment.
* Task priorities.
* Due dates.
* Task status.
* Kanban workflow.

Kanban states:

```text
To Do → In Progress → Review → Done
```

## Chat

Responsible for:

* Workspace communication.
* Real-time messaging.
* Message persistence.
* Online presence.
* Image messages.
* Message editing and deletion.

Socket.io handles real-time delivery while MongoDB stores message data.

## Files

Responsible for:

* File validation.
* Uploading.
* File metadata.
* File previews.
* Authorized downloads.
* File deletion.

Large file content is stored in cloud storage rather than MongoDB.

## Notifications

Responsible for:

* Task notifications.
* Message notifications.
* Project notifications.
* Invitation notifications.
* File-related notifications.
* Read/unread status.

## Dashboard

Responsible for:

* Workspace summaries.
* Project/task counts.
* Recent activity.
* Recent projects.
* Upcoming tasks.
* Notification summaries.

## Audit

Audit logging is optional for the MVP.

When enabled, it may record:

* Task creation.
* Project updates.
* Invitations.
* File uploads.
* Security events.
* Administrative actions.

---

# 8. Communication and Request Flow

REST API is used for standard data operations such as:

* Authentication.
* Workspace CRUD.
* Project CRUD.
* Task CRUD.
* File metadata.
* Notification history.
* Dashboard data.

WebSockets are used for:

* Chat messages.
* Online presence.
* Real-time task updates.
* Live notifications.

## REST Request Flow

```text
Browser
   │
   ▼
Next.js
   │
   ▼
Express Route
   │
   ▼
Authentication
   │
   ▼
Authorization
   │
   ▼
Validation
   │
   ▼
Controller
   │
   ▼
Service
   │
   ▼
Mongoose
   │
   ▼
MongoDB
   │
   ▼
Response
```

## Real-Time Request Flow

```text
Browser
   │
   ▼
Socket.io
   │
   ▼
Socket Authentication
   │
   ▼
Room / Membership Check
   │
   ▼
Business Service
   │
   ▼
MongoDB
   │
   ▼
Broadcast
   │
   ▼
Authorized Workspace Members
```

---

# 9. Project Structure

## Backend

```text
backend/src/
├── config/
├── middleware/
├── modules/
│   ├── auth/
│   ├── users/
│   ├── workspaces/
│   ├── projects/
│   ├── tasks/
│   ├── messages/
│   ├── files/
│   └── notifications/
├── models/
├── sockets/
├── utils/
└── types/
```

## Frontend

```text
frontend/src/
├── app/
├── components/
├── features/
├── lib/
├── hooks/
├── stores/
├── types/
└── styles/
```

Feature-specific frontend functionality should be grouped inside `features/` rather than placing all components in one directory.

---

# 10. Security Architecture

HiberTech security is enforced primarily at the backend.

The system shall:

* Hash passwords using bcrypt or Argon2.
* Never store plain-text passwords.
* Protect private routes with authentication middleware.
* Enforce role-based authorization.
* Check workspace membership on protected operations.
* Validate and sanitize request data.
* Use short-lived access tokens.
* Use secure password-reset tokens.
* Use HTTPS in production.
* Configure CORS carefully.
* Apply rate limiting to authentication endpoints.
* Validate file type and size.
* Restrict private file access.
* Avoid exposing internal error details.
* Record important security and system activities where audit logging is enabled.

### Security Boundary

```text
┌─────────────────────────────────────┐
│            USER BROWSER             │
│                                     │
│  UI validation                      │
│  Token handling                     │
└──────────────────┬──────────────────┘
                   │ HTTPS / WSS
                   ▼
┌─────────────────────────────────────┐
│             BACKEND                 │
│                                     │
│ Authentication                      │
│ Authorization                       │
│ Workspace membership checks         │
│ Input validation                    │
│ Business rules                      │
│ File authorization                  │
└──────────────────┬──────────────────┘
                   │
          ┌────────┴────────┐
          ▼                 ▼
     MongoDB            Cloud Storage
```

Frontend validation improves user experience, but **must never be treated as the primary security boundary**.

---

# 11. Deployment Architecture

| Layer          | Recommended Platform | Purpose                     |
| -------------- | -------------------- | --------------------------- |
| Frontend       | Vercel               | Host Next.js application    |
| Backend        | Render or Railway    | Run Express and Socket.io   |
| Database       | MongoDB Atlas        | Application data            |
| File Storage   | Cloudinary or AWS S3 | Uploaded files              |
| Email          | Email provider       | Reset and invitation emails |
| Source Control | GitHub               | Code and documentation      |

Deployment structure:

```text
                         Internet
                            │
                ┌───────────┴───────────┐
                ▼                       ▼
             Vercel                  Backend
           Next.js App          Render / Railway
                │                       │
                │                 ┌─────┼─────┐
                │                 ▼     ▼     ▼
                │              MongoDB Cloud Email
                │              Atlas   Storage Service
                │
                └──────── HTTPS / WebSocket ────────┘
```

---

# 12. Environment Configuration

## Frontend

```env
NEXT_PUBLIC_API_URL=https://api.example.com/api/v1
NEXT_PUBLIC_SOCKET_URL=https://api.example.com
```

## Backend

```env
NODE_ENV=production
PORT=5000
MONGODB_URI=<connection-string>
JWT_SECRET=<secure-secret>
JWT_EXPIRES_IN=15m
CLIENT_URL=https://app.example.com
CLOUDINARY_URL=<storage-connection>
EMAIL_PROVIDER_API_KEY=<provider-key>
```

Secrets must never be committed to GitHub.

A `.env.example` file should contain variable names without exposing actual credentials.

---

# 13. Scalability and Reliability

## MVP Architecture

The initial deployment should use:

* One Express backend.
* One Socket.io server.
* MongoDB Atlas.
* Cloud file storage.
* Database indexes.
* Feature-based modules.

The MVP should avoid unnecessary infrastructure complexity.

## Reliability

The system should:

* Reconnect Socket.io clients automatically.
* Retry safe operations where appropriate.
* Preserve unsent messages where practical.
* Show upload progress.
* Provide upload retry options.
* Maintain database backups.
* Maintain documented rollback procedures.
* Handle temporary network/server failures gracefully.

## Future Scaling

When justified by actual usage, the architecture may introduce:

* Redis.
* Socket.io Redis adapter.
* Background job queues.
* Search services.
* Read replicas.
* CDN.
* Independent services.
* Dedicated analytics infrastructure.

Scaling components should be introduced based on measured system requirements rather than prematurely.

---

# 14. Future Architecture

HiberTech is intentionally designed so that future services can be added without replacing the MVP architecture.

```text
                         HiberTech
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              ▼              ▼
           Current         Future         Future
           Services        Services       Clients
              │              │              │
       ┌──────┼──────┐   ┌───┼────┐      ┌──┴────┐
       ▼      ▼      ▼   ▼   ▼    ▼      ▼       ▼
    Mongo  Socket  Cloud Redis Jobs Search Mobile  Web
    DB     .io     Storage      Queue Service App
                             │
                             ▼
                       Analytics
                             │
                             ▼
                            AI
```

Potential future components include:

### Redis

For caching, distributed sessions, and Socket.io scaling.

### Background Jobs

For:

* Email processing.
* Notifications.
* File processing.
* Scheduled tasks.

### Search Service

For advanced workspace, project, message, and file searching.

### Analytics Service

For advanced productivity and organization analytics.

### AI Service

For future AI-powered:

* Task assistance.
* Project summaries.
* Productivity insights.
* Search assistance.
* Automated recommendations.

### Mobile Application

A future mobile client can consume the same REST API and real-time services.

---

# 15. Key Architecture Decisions

## Modular Monolith

**Decision:** Use a modular monolith for v1.

**Reason:** Simple development, deployment, testing, and debugging for a small MVP team.

## REST + WebSockets

**Decision:** Use REST and Socket.io together.

**Reason:** REST is appropriate for predictable CRUD operations, while Socket.io provides real-time collaboration.

## MongoDB

**Decision:** Use MongoDB with Mongoose.

**Reason:** Fits the planned Node.js stack and provides a flexible document model for collaboration data.

## External File Storage

**Decision:** Store files externally and metadata in MongoDB.

**Reason:** Prevents large file content from unnecessarily increasing database storage requirements.

## Server-Side Security

**Decision:** Enforce sensitive permissions and validation on the backend.

**Reason:** Frontend controls can be bypassed and must not be trusted as a security boundary.

---

# 16. Architecture Constraints

The MVP architecture must respect the following constraints:

* Small development team.
* Limited MVP timeline.
* Web-first product.
* Essential features prioritized.
* Minimal infrastructure complexity.
* Secure handling of user and workspace data.
* Architecture must support desktop and mobile browsers.
* Advanced features remain outside the MVP.

---

# 17. Technology Specification

| Item             | Specification                           |
| ---------------- | --------------------------------------- |
| Frontend         | Next.js + React + TypeScript            |
| Backend          | Node.js + Express.js                    |
| API              | REST API                                |
| Real-Time        | Socket.io / WebSocket                   |
| Database         | MongoDB                                 |
| ODM              | Mongoose                                |
| Authentication   | JWT                                     |
| Password Hashing | bcrypt or Argon2                        |
| File Storage     | Cloudinary or AWS S3                    |
| Email            | Email provider                          |
| Base API URL     | `/api/v1`                               |
| Deployment       | Vercel + Render/Railway + MongoDB Atlas |

---

# 18. Definition of Done

The architecture documentation is considered complete when:

* Major system components and responsibilities are defined.
* REST and WebSocket communication are separated.
* Data and request flows are documented.
* Security boundaries are defined.
* Deployment components are identified.
* Frontend and backend folder structures are agreed upon.
* Architecture principles are documented.
* Future architecture direction is documented.
* The development team approves the architecture.

---

# 19. Architecture Summary

The HiberTech MVP follows this architecture:

```text
                    HIBERTECH MVP
                         │
             ┌───────────┴───────────┐
             │                       │
             ▼                       ▼
        Next.js Web              Socket.io
             │                       │
             ▼                       ▼
        REST API              Real-Time Events
             │                       │
             └───────────┬───────────┘
                         ▼
                  Express Backend
                         │
             ┌───────────┼───────────┐
             ▼           ▼           ▼
        Business      Auth /      Services
        Modules       RBAC            │
             │                        │
             └──────────┬─────────────┘
                        ▼
                     MongoDB
                        │
                 ┌──────┴──────┐
                 ▼             ▼
            Cloud Storage   Email Service
```

**Core architectural principle:**

> **Keep the MVP simple, secure, modular, and scalable enough to evolve into a larger SaaS platform.**
