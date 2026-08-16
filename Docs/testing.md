# HiberTech — Testing

## 1. Purpose

This document defines the testing strategy for HiberTech.

The goal is to ensure that all MVP features work correctly, securely,
reliably, and consistently across supported environments.

Testing should identify functional defects, security problems,
integration issues, and usability problems before release.

---

## 2. Testing Goals

- Verify that functional requirements are implemented correctly.
- Verify that user stories and acceptance criteria are satisfied.
- Verify API behavior and validation.
- Verify authentication and authorization.
- Verify real-time communication.
- Verify file upload and download behavior.
- Verify responsive UI behavior.
- Prevent regressions when new features are added.
- Ensure the MVP is stable before deployment.

---

## 3. Testing Types

### 3.1 Unit Testing

Tests individual functions, utilities, services, and business rules
in isolation.

Examples:

- Password validation
- Task status validation
- Permission checks
- Notification creation
- Data transformation utilities

---

### 3.2 Integration Testing

Tests interactions between application modules and external dependencies.

Examples:

- Controller → Service → Database
- Authentication → User database
- Project → Task relationship
- Workspace → Member relationship
- Notification creation after task assignment

---

### 3.3 API Testing

Tests REST API endpoints for correct requests, responses,
authentication, authorization, validation, and error handling.

Key areas:

- Authentication
- Users
- Workspaces
- Invitations
- Projects
- Tasks
- Kanban
- Messages
- Files
- Notifications
- Dashboard
- Admin

---

### 3.4 UI Testing

Tests frontend screens and reusable components.

Key areas:

- Forms
- Buttons
- Navigation
- Modals
- Tables
- Kanban board
- Chat interface
- File interface
- Notifications
- Responsive layouts

---

### 3.5 End-to-End Testing

Tests complete user workflows from beginning to end.

Examples:

- Register → Login → Dashboard
- Create Workspace → Invite Member → Accept Invitation
- Create Project → Create Task → Assign Task
- Move Task → Verify Status Update
- Open Chat → Send Message → Receive Message
- Upload File → Preview → Download

---

### 3.6 Security Testing

Security testing verifies that protected resources cannot be accessed
without the required permissions.

Examples:

- Invalid login credentials
- Expired authentication token
- Unauthorized workspace access
- Unauthorized project access
- Unauthorized task modification
- Role permission violations
- Invalid file uploads
- Malicious input
- Rate-limit behavior

---

## 4. Key Testing Areas

The following MVP modules must be tested:

- Authentication
- User Profiles
- Workspaces
- Workspace Members
- Invitations
- Projects
- Tasks
- Kanban Board
- Real-Time Chat
- File Sharing
- Notifications
- Dashboard
- Access Control
- Administration

---

## 5. Test Case Format

Each test case should contain:

- Test Case ID
- Feature
- Description
- Preconditions
- Test Steps
- Expected Result
- Actual Result
- Status
- Priority

---

## 6. Example Test Cases

### TC-001 — Login With Valid Credentials

**Feature:** Authentication

**Given:**

A registered user exists.

**When:**

The user enters valid email and password credentials.

**Then:**

The system authenticates the user and redirects the user to
the dashboard.

**Expected Result:**

The dashboard is displayed and protected resources become accessible.

**Priority:** High

---

### TC-002 — Login With Invalid Password

**Feature:** Authentication

**Given:**

A registered user exists.

**When:**

The user enters an incorrect password.

**Then:**

The login request is rejected.

**Expected Result:**

A safe authentication error is displayed without exposing
sensitive information.

**Priority:** High

---

### TC-003 — Create Workspace

**Feature:** Workspace

**Given:**

The user is authenticated.

**When:**

The user submits valid workspace information.

**Then:**

A new workspace is created.

**Expected Result:**

The user becomes an authorized member/owner of the workspace
and the workspace appears in the workspace list.

**Priority:** High

---

### TC-004 — Create Task

**Feature:** Tasks

**Given:**

The user has permission to create tasks in a project.

**When:**

The user submits valid task information.

**Then:**

The task is created inside the selected project.

**Expected Result:**

The new task appears in the project task list and Kanban board.

**Priority:** High

---

### TC-005 — Move Task on Kanban

**Feature:** Kanban

**Given:**

A task exists in the `todo` column.

**When:**

An authorized user moves the task to `in-progress`.

**Then:**

The task status changes to `in-progress`.

**Expected Result:**

The Kanban board displays the task in the correct column.

**Priority:** High

---

### TC-006 — Send Real-Time Message

**Feature:** Chat

**Given:**

Two authorized users are members of the same workspace.

**When:**

One user sends a chat message.

**Then:**

The message is delivered to the authorized workspace members
in real time.

**Expected Result:**

The recipient sees the message without manually refreshing the page.

**Priority:** High

---

### TC-007 — Unauthorized Workspace Access

**Feature:** Security

**Given:**

A user does not belong to a workspace.

**When:**

The user attempts to access the workspace using its ID.

**Then:**

The server rejects the request.

**Expected Result:**

The user receives a `403 Forbidden` or appropriate authorization response.

**Priority:** Critical

---

### TC-008 — File Upload Validation

**Feature:** Files

**Given:**

A user is authorized to upload files.

**When:**

The user attempts to upload an unsupported or oversized file.

**Then:**

The upload is rejected.

**Expected Result:**

A clear validation error is returned and the invalid file is not stored.

**Priority:** High

---

## 7. Acceptance Criteria Testing

User stories must be tested against their acceptance criteria.

A user story is considered complete when:

- Its acceptance criteria pass.
- Required API behavior works.
- Required UI behavior works.
- Authorization rules are enforced.
- Error and empty states are handled.
- Relevant automated tests pass.

---

## 8. API Testing Standards

API tests should verify:

- HTTP method
- Endpoint
- Authentication
- Authorization
- Request validation
- Response status
- Response structure
- Error handling
- Database changes

Important status codes include:

- `200` — Successful request
- `201` — Resource created
- `204` — Successful operation without response content
- `400` — Bad request
- `401` — Unauthorized
- `403` — Forbidden
- `404` — Not found
- `409` — Conflict
- `422` — Validation error
- `500` — Internal server error

---

## 9. Real-Time Testing

Socket.io features should be tested for:

- Connection authentication
- Workspace room authorization
- Message delivery
- Online presence
- Task updates
- Notification delivery
- Reconnection behavior
- Unauthorized room access

---

## 10. Responsive Testing

The application should be tested on:

- Desktop
- Tablet
- Mobile

Important areas include:

- Navigation
- Dashboard
- Kanban board
- Chat
- File management
- Forms
- Modals
- Tables and lists

---

## 11. Regression Testing

Regression tests should be executed when major features or shared
components are changed.

Particular attention should be given to:

- Authentication
- Authorization
- Workspace isolation
- Task status changes
- Real-time communication
- File access
- Shared UI components

---

## 12. Test Environments

### Development

Used for local development and feature testing.

### Staging

Used for integration, regression, and release testing.

### Production

Used only for final verification and monitoring after deployment.

Production testing must avoid modifying real user data.

---

## 13. Definition of Done

A feature is considered tested when:

- Unit tests pass where applicable.
- Integration tests pass where applicable.
- API behavior is verified.
- UI behavior is verified.
- Acceptance criteria pass.
- Security and authorization checks pass.
- Responsive behavior is verified.
- Critical regression tests pass.
- Known defects are documented.

---

## 14. Future Testing Improvements

As HiberTech grows, testing may be expanded with:

- Automated CI/CD test execution
- Performance testing
- Load testing
- Security scanning
- Accessibility testing
- Browser compatibility testing
- Visual regression testing
- Test coverage reporting