# HiberTech — Release Management

## 1. Purpose

This document defines the release progression for HiberTech MVP.
Each release represents a major development milestone and adds a
specific set of validated features.

## 2. Release Strategy

HiberTech will use incremental releases during MVP development.

Each release should:

- Complete its planned functionality.
- Pass relevant tests.
- Meet security requirements.
- Be reviewed before moving to the next milestone.
- Avoid introducing features outside the approved MVP scope.

## 3. Release Versions

### v0.1 — Project Foundation

Initial project setup, repository structure, development environment,
frontend and backend foundations, database connection, and basic
application configuration.

### v0.2 — Authentication

- User registration
- Login
- Logout
- Password reset
- JWT authentication
- Protected routes

### v0.3 — Workspace Management

- Workspace creation
- Workspace listing
- Member invitations
- Invitation acceptance
- Member management
- Workspace authorization

### v0.4 — Projects

- Project creation
- Project listing
- Project details
- Project editing
- Project deletion

### v0.5 — Tasks and Kanban

- Task creation
- Task assignment
- Priority
- Due dates
- Task status
- Kanban board
- Task movement between columns

### v0.6 — Real-Time Chat

- Workspace chat
- Real-time messaging
- Online status
- Emoji messages
- Image messages

### v0.7 — Files

- File upload
- File listing
- File download
- File preview
- File deletion
- File authorization

### v0.8 — Notifications

- Task notifications
- Message notifications
- Project notifications
- Notification list
- Mark as read
- Mark all as read

### v0.9 — Beta

MVP features are integrated and tested together.

Beta activities include:

- End-to-end testing
- Security testing
- UI/UX validation
- Performance checks
- Bug fixing
- Early-user feedback
- Deployment validation

### v1.0 — MVP Release

The first stable HiberTech MVP release.

The release must include all approved MVP functionality and satisfy
the project's functional, security, usability, reliability, and
deployment requirements.

## 4. Release Criteria

A release may proceed when:

- Planned features are implemented.
- Critical tests pass.
- No known critical security issues remain.
- Major bugs are resolved.
- API and UI behavior are validated.
- Documentation is updated.
- The release is approved by the development team.

## 5. Future Releases

Features outside the MVP will be considered for future versions,
including:

- AI assistance
- Video meetings
- Voice communication
- Calendar integration
- Third-party integrations
- Advanced analytics
- Billing and payments
- Mobile applications
- Advanced SaaS capabilities