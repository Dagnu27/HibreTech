# HiberTech — API Design

## 1. Purpose

This document defines the REST API architecture for HiberTech v1.0.

The API provides backend services for authentication, users, workspaces, projects, tasks, Kanban boards, messaging, file sharing, notifications, dashboards, and administration.

The API is designed to be modular, secure, consistent, and extensible for future SaaS development.

---

## 2. API Architecture

HiberTech uses a RESTful API architecture.

### Base URL

```text
/api/v1
```

Example:

```text
POST /api/v1/auth/login
```

The API uses standard HTTP methods:

| Method | Purpose                     |
| ------ | --------------------------- |
| GET    | Retrieve resources          |
| POST   | Create resources            |
| PUT    | Replace or update resources |
| PATCH  | Partially update resources  |
| DELETE | Delete resources            |

---

## 3. API Module Hierarchy

```text
HiberTech REST API
│
├── Authentication
├── Users
├── Workspaces
│   └── Members & Invitations
├── Projects
├── Tasks
│   └── Kanban Board
├── Messages
├── Files
├── Notifications
├── Dashboard
├── Admin
└── Audit Logs
```

---

# 4. API Standards

## 4.1 Response Format

Successful responses should follow a consistent structure:

```json
{
  "success": true,
  "data": {}
}
```

For collection responses:

```json
{
  "success": true,
  "data": [],
  "pagination": {}
}
```

## 4.2 Error Format

Errors should follow a consistent structure:

```json
{
  "success": false,
  "message": "Invalid request",
  "error": "ValidationError"
}
```

Sensitive implementation details such as stack traces, database errors, passwords, tokens, or internal service information must not be exposed.

---

# 5. Authentication API

| Operation       | Method | Endpoint                | Purpose                               |
| --------------- | ------ | ----------------------- | ------------------------------------- |
| Register        | POST   | `/auth/register`        | Create a new account                  |
| Login           | POST   | `/auth/login`           | Authenticate a user and return a JWT  |
| Logout          | POST   | `/auth/logout`          | End the current authenticated session |
| Forgot Password | POST   | `/auth/forgot-password` | Request password reset                |
| Reset Password  | POST   | `/auth/reset-password`  | Set a new password                    |

### Register Example

```http
POST /api/v1/auth/register
```

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "Password123"
}
```

---

# 6. User API

| Operation        | Method | Endpoint             | Purpose                               |
| ---------------- | ------ | -------------------- | ------------------------------------- |
| Get Current User | GET    | `/users/me`          | Return authenticated user information |
| View Profile     | GET    | `/users/:userId`     | View a user's profile                 |
| Update Profile   | PUT    | `/users/me`          | Update authenticated user's profile   |
| Change Password  | PUT    | `/users/me/password` | Change authenticated user's password  |

---

# 7. Workspace API

| Operation           | Method | Endpoint                   | Purpose                          |
| ------------------- | ------ | -------------------------- | -------------------------------- |
| Create Workspace    | POST   | `/workspaces`              | Create a workspace               |
| Get User Workspaces | GET    | `/workspaces`              | List user's available workspaces |
| Get Workspace       | GET    | `/workspaces/:workspaceId` | Return workspace details         |
| Update Workspace    | PUT    | `/workspaces/:workspaceId` | Update workspace information     |
| Delete Workspace    | DELETE | `/workspaces/:workspaceId` | Delete a workspace               |

### Create Workspace

```http
POST /api/v1/workspaces
```

```json
{
  "name": "HiberTech Development",
  "description": "Software development workspace"
}
```

---

# 8. Workspace Members and Invitations

| Operation         | Method | Endpoint                                   | Purpose                     |
| ----------------- | ------ | ------------------------------------------ | --------------------------- |
| Get Members       | GET    | `/workspaces/:workspaceId/members`         | List workspace members      |
| Invite Member     | POST   | `/workspaces/:workspaceId/invitations`     | Invite a user by email      |
| Accept Invitation | POST   | `/invitations/:token/accept`               | Accept workspace invitation |
| Remove Member     | DELETE | `/workspaces/:workspaceId/members/:userId` | Remove workspace member     |

Only authorized workspace owners, administrators, or managers may perform member-management operations according to the application's authorization rules.

---

# 9. Project API

| Operation      | Method | Endpoint                            | Purpose                    |
| -------------- | ------ | ----------------------------------- | -------------------------- |
| Create Project | POST   | `/workspaces/:workspaceId/projects` | Create a project           |
| Get Projects   | GET    | `/workspaces/:workspaceId/projects` | List workspace projects    |
| Get Project    | GET    | `/projects/:projectId`              | Return project details     |
| Update Project | PUT    | `/projects/:projectId`              | Update project information |
| Delete Project | DELETE | `/projects/:projectId`              | Delete a project           |

---

# 10. Task API

| Operation   | Method | Endpoint                     | Purpose                 |
| ----------- | ------ | ---------------------------- | ----------------------- |
| Create Task | POST   | `/projects/:projectId/tasks` | Create a task           |
| Get Tasks   | GET    | `/projects/:projectId/tasks` | List project tasks      |
| Get Task    | GET    | `/tasks/:taskId`             | Return task details     |
| Update Task | PUT    | `/tasks/:taskId`             | Update task information |
| Delete Task | DELETE | `/tasks/:taskId`             | Delete a task           |

### Create Task

```http
POST /api/v1/projects/:projectId/tasks
```

```json
{
  "title": "Create login page",
  "description": "Build the login interface",
  "assignedTo": "USER_ID",
  "priority": "high",
  "dueDate": "2026-08-20"
}
```

---

# 11. Kanban Board API

| Operation | Method | Endpoint                     | Purpose                       |
| --------- | ------ | ---------------------------- | ----------------------------- |
| Get Board | GET    | `/projects/:projectId/board` | Return project's Kanban board |
| Move Task | PATCH  | `/tasks/:taskId/status`      | Move task between statuses    |

### Move Task

```http
PATCH /api/v1/tasks/:taskId/status
```

```json
{
  "status": "in-progress"
}
```

Allowed statuses:

```text
todo
in-progress
review
done
```

These correspond to the business workflow:

```text
To Do → In Progress → Review → Done
```

---

# 12. Messaging API

| Operation      | Method | Endpoint                            | Purpose                  |
| -------------- | ------ | ----------------------------------- | ------------------------ |
| Get Messages   | GET    | `/workspaces/:workspaceId/messages` | List workspace messages  |
| Send Message   | POST   | `/workspaces/:workspaceId/messages` | Send a workspace message |
| Edit Message   | PUT    | `/messages/:messageId`              | Edit a message           |
| Delete Message | DELETE | `/messages/:messageId`              | Delete a message         |

### Send Message

```http
POST /api/v1/workspaces/:workspaceId/messages
```

```json
{
  "content": "The project is ready for review.",
  "type": "text"
}
```

Real-time message delivery will be implemented using Socket.IO while the REST API handles message persistence and retrieval.

---

# 13. File API

| Operation         | Method | Endpoint                         | Purpose                 |
| ----------------- | ------ | -------------------------------- | ----------------------- |
| Upload File       | POST   | `/workspaces/:workspaceId/files` | Upload a file           |
| Get Files         | GET    | `/workspaces/:workspaceId/files` | List workspace files    |
| Get Project Files | GET    | `/projects/:projectId/files`     | List project files      |
| Download File     | GET    | `/files/:fileId/download`        | Download a file         |
| Preview File      | GET    | `/files/:fileId/preview`         | Preview supported files |
| Delete File       | DELETE | `/files/:fileId`                 | Delete a file           |

Uploaded files must be validated for file type and size before storage.

---

# 14. Notification API

| Operation                | Method | Endpoint                              | Purpose                        |
| ------------------------ | ------ | ------------------------------------- | ------------------------------ |
| Get Notifications        | GET    | `/notifications`                      | List user notifications        |
| Get Unread Notifications | GET    | `/notifications/unread`               | List unread notifications      |
| Mark as Read             | PATCH  | `/notifications/:notificationId/read` | Mark one notification as read  |
| Mark All as Read         | PATCH  | `/notifications/read-all`             | Mark all notifications as read |

---

# 15. Dashboard API

| Operation     | Method | Endpoint     | Purpose                              |
| ------------- | ------ | ------------ | ------------------------------------ |
| Get Dashboard | GET    | `/dashboard` | Return dashboard summary information |

The dashboard may include:

* Recent projects
* Recent tasks
* Task statistics
* Recent activity
* Notifications
* Workspace summary

---

# 16. Admin API

| Operation      | Method | Endpoint               | Purpose                  |
| -------------- | ------ | ---------------------- | ------------------------ |
| Get Users      | GET    | `/admin/users`         | List users               |
| Get User       | GET    | `/admin/users/:userId` | View a user              |
| Update User    | PUT    | `/admin/users/:userId` | Update a user            |
| Delete User    | DELETE | `/admin/users/:userId` | Delete a user            |
| Get Statistics | GET    | `/admin/statistics`    | Return system statistics |

All Admin API endpoints require appropriate administrator authorization.

---

# 17. Audit Log API

Audit logging is optional/future functionality for the MVP.

| Operation      | Method | Endpoint                   | Purpose            |
| -------------- | ------ | -------------------------- | ------------------ |
| Get Audit Logs | GET    | `/admin/audit-logs`        | List audit logs    |
| Get Audit Log  | GET    | `/admin/audit-logs/:logId` | View one audit log |

---

# 18. Authentication and Authorization

Protected endpoints require a JWT access token.

```http
Authorization: Bearer <JWT_TOKEN>
```

### Role Permissions

| Role    | Main Permissions                                            |
| ------- | ----------------------------------------------------------- |
| Admin   | Full system administration                                  |
| Manager | Manage projects, tasks, and authorized workspace operations |
| Member  | Use permitted workspace features and work on assigned tasks |
| Guest   | Register and log in; no protected workspace access          |

Authorization must be checked on the backend rather than relying only on frontend restrictions.

---

# 19. Pagination

Collection endpoints should support pagination.

Example:

```http
GET /api/v1/projects?page=1&limit=10
```

```http
GET /api/v1/tasks?page=1&limit=20
```

Example response:

```json
{
  "success": true,
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "totalPages": 5
  }
}
```

Default and maximum `limit` values should be defined during implementation to prevent excessive queries.

---

# 20. Filtering

List endpoints should support appropriate filtering.

### Tasks by status

```http
GET /api/v1/tasks?status=todo
```

### Tasks by priority

```http
GET /api/v1/tasks?priority=high
```

### Projects by search term

```http
GET /api/v1/projects?search=website
```

Filtering must only return resources the authenticated user is authorized to access.

---

# 21. Sorting

Collection endpoints may support sorting.

Example:

```http
GET /api/v1/tasks?sortBy=dueDate&order=asc
```

Example:

```http
GET /api/v1/projects?sortBy=createdAt&order=desc
```

Supported sorting fields should be explicitly defined by each endpoint during implementation.

---

# 22. HTTP Status Codes

| Status Code | Meaning                                       |
| ----------- | --------------------------------------------- |
| 200         | Successful request                            |
| 201         | Resource created                              |
| 204         | Successful operation with no response content |
| 400         | Bad request                                   |
| 401         | Unauthorized                                  |
| 403         | Forbidden                                     |
| 404         | Resource not found                            |
| 409         | Conflict                                      |
| 422         | Validation error                              |
| 500         | Internal server error                         |

---

# 23. Security Requirements

The API shall:

* Use JWT authentication for protected endpoints.
* Store only securely hashed passwords.
* Never store raw passwords.
* Validate request bodies and parameters.
* Enforce workspace membership.
* Enforce role-based authorization.
* Validate uploaded file type and size.
* Sanitize user-controlled input.
* Use secure cloud-storage access for uploaded files.
* Avoid exposing sensitive implementation details in error responses.
* Prevent unauthorized access through direct API requests.
* Apply appropriate validation before database operations.

---

# 24. Backend Project Structure

```text
src/
├── controllers/
│   ├── auth.controller.js
│   ├── user.controller.js
│   ├── workspace.controller.js
│   ├── project.controller.js
│   ├── task.controller.js
│   ├── message.controller.js
│   ├── file.controller.js
│   ├── notification.controller.js
│   └── dashboard.controller.js
│
├── models/
│   ├── User.js
│   ├── Workspace.js
│   ├── Project.js
│   ├── Task.js
│   ├── Message.js
│   ├── Notification.js
│   └── File.js
│
├── routes/
│   ├── auth.routes.js
│   ├── user.routes.js
│   ├── workspace.routes.js
│   ├── project.routes.js
│   ├── task.routes.js
│   ├── message.routes.js
│   ├── file.routes.js
│   ├── notification.routes.js
│   └── dashboard.routes.js
│
├── middleware/
│   ├── auth.middleware.js
│   ├── role.middleware.js
│   ├── validation.middleware.js
│   └── error.middleware.js
│
├── services/
│   ├── auth.service.js
│   ├── email.service.js
│   ├── file.service.js
│   └── notification.service.js
│
├── validations/
│   ├── auth.validation.js
│   ├── workspace.validation.js
│   ├── project.validation.js
│   └── task.validation.js
│
└── app.js
```

---

# 25. API Layer Architecture

```text
Client
  │
  ▼
REST API
  │
  ▼
Routes
  │
  ▼
Middleware
  │
  ├── Authentication
  ├── Authorization
  ├── Validation
  └── Error Handling
  │
  ▼
Controllers
  │
  ▼
Services
  │
  ▼
Models
  │
  ▼
MongoDB
```

Real-time communication follows an additional path:

```text
Client
  │
  ▼
Socket.IO
  │
  ▼
Real-Time Events
  │
  ▼
Message / Notification Services
  │
  ▼
MongoDB
```

---

# 26. API Requirements Traceability

The API design supports the following documentation:

| API Module     | Related Documentation                   |
| -------------- | --------------------------------------- |
| Authentication | Requirements + User Stories + Use Cases |
| Users          | Requirements + User Stories             |
| Workspaces     | Requirements + Business Rules           |
| Projects       | Requirements + User Stories             |
| Tasks          | Requirements + Business Rules           |
| Kanban         | User Stories + Business Rules           |
| Messages       | Requirements + User Stories             |
| Files          | Requirements + Business Rules           |
| Notifications  | Requirements + User Stories             |
| Dashboard      | Requirements + User Stories             |
| Admin          | User Roles + Access Control             |
| Audit Logs     | Database Design + Future Security       |

---

# 27. Future API Extensions

The following APIs are outside HiberTech v1.0 and may be introduced in future versions:

* AI Assistant API
* Video Meeting API
* Voice Communication API
* Calendar API
* Third-Party Integration API
* Billing and Payment API
* Advanced Analytics API
* Mobile Application API

These should not be implemented during the MVP unless the project scope is formally expanded.

---

# 28. Implementation Notes

The following details will be finalized during implementation:

* Exact validation rules
* JWT expiration time
* Refresh-token strategy
* Password-reset token expiration
* Pagination defaults and maximum limits
* Rate limiting
* File-size limits
* Supported file types
* Socket.IO event naming
* API request logging
* API versioning strategy beyond v1
* Caching strategy

The API should maintain backward compatibility within the v1 API wherever practical.

---

## 29. API Design Summary

HiberTech v1.0 provides a modular REST API covering the complete MVP workflow:

```text
Authentication
      ↓
Workspace
      ↓
Project
      ↓
Task
      ↓
Kanban
      ↓
Communication
      ↓
Files
      ↓
Notifications
      ↓
Dashboard
      ↓
Administration
```

The API design follows the project's core principle:

**Simple MVP first → secure architecture → scalable SaaS foundation.**
