HiberTech — Database Design
1. Purpose
This document defines the database structure for HiberTech v1.0, including the main collections, important fields, relationships, and data ownership rules.
The database design supports the requirements and user stories defined in:
docs/02-requirements.md
docs/03-user-stories.md
docs/04-use-cases.md
docs/05-business-rules.md
The database is designed around the core HiberTech workflow:
Users → Workspaces → Projects → Tasks → Chat / Files → Notifications
2. Database Technology
Primary Database: MongoDB
HiberTech uses a document-oriented database model. MongoDB is suitable for the MVP because the platform contains relationships between users, workspaces, projects, tasks, messages, files, and notifications while also requiring flexible document structures.
3. Collections
The primary HiberTech collections are:
Users
Workspaces
Projects
Tasks
Messages
Files
Notifications
AuditLogs — optional for MVP
Invitations — future/optional dedicated collection
3.1 Users
Stores user account, authentication, profile, and workspace membership information.
Main fields:
_id
name
email
passwordHash
avatarUrl
role
workspaceIds
isEmailVerified
lastLoginAt
createdAt
updatedAt

Role values:
Admin
Manager
Member
3.2 Workspaces
Stores team workspace information and workspace membership.


Main fields:

_id
name
description
ownerId
memberIds
pendingInvites
isActive
createdAt
updatedAt

The current ERD represents workspace membership through references between users and workspaces rather than a separate WorkspaceMember collection.

3.3 Projects
Stores projects belonging to a workspace.
Main fields:

_id
workspaceId
name
description
status
createdBy
memberIds
dueDate
isArchived
createdAt
updatedAt

Supported project status values may include:

Planning
Active
On Hold
Completed
Archived
3.4 Tasks
Stores individual work items belonging to projects.
Main fields:

_id
projectId
title
description
type
assignedTo
createdBy
priority
status
dueDate
completedAt
tags
attachments
createdAt
updatedAt

Supported task priorities:

Low
Medium
High
Urgent

Supported Kanban statuses:
To Do
In Progress
Review
Done
3.5 Messages
Stores workspace chat messages.
Main fields:

_id
workspaceId
senderId
content
type
fileUrl
replyTo
isEdited
isDeleted
createdAt
updatedAt

Supported message types may include:
Text
Image
Emoji
File
3.6 Files
Stores metadata for files uploaded to HiberTech.

Main fields:

_id
workspaceId
projectId
uploadedBy
fileName
fileType
fileSize
fileUrl
thumbnailUrl
isDeleted
createdAt
updatedAt

The actual file content should be stored using appropriate cloud/object storage rather than directly inside MongoDB.

3.7 Notifications
Stores notifications generated for users.
Main fields:

_id
userId
type
title
message
relatedEntityId
relatedEntityType
isRead
createdAt
readAt

Possible notification types include:

Task Assigned
Task Updated
New Message
Project Updated
Invitation
3.8 AuditLogs
Audit logs are optional for the initial MVP but provide a foundation for security and administrative monitoring.

Main fields:

_id
userId
action
entityType
entityId
metadata
ipAddress
userAgent
createdAt

Possible actions include:

User Login
Task Created
Task Updated
Project Created
Project Deleted
Member Removed
3.9 Invitations

The current ERD represents pending invitations inside the Workspaces document through pendingInvites.
For the MVP, this approach can remain simple.
If invitation functionality becomes more complex, a dedicated Invitations collection can be introduced.

Possible fields:

_id
workspaceId
email
invitedBy
token
status
expiresAt
createdAt
acceptedAt

This collection is therefore considered future/optional, not a required separate collection for HiberTech v1.0.

4. Relationships
User → Workspace
A user can belong to multiple workspaces.
A workspace can contain multiple users.
Relationship:
User N ↔ N Workspace
The current design represents this using:
Users.workspaceIds
Workspaces.memberIds
A workspace also contains an ownerId identifying its owner.
Workspace → Project
A workspace can contain multiple projects.
Each project belongs to one workspace.
Relationship:
Workspace 1 → N Projects
Project → Task
A project can contain multiple tasks.
Each task belongs to one project.
Relationship:
Project 1 → N Tasks
Workspace → Message
A workspace can contain many messages.
Each message belongs to a workspace and references its sender.
Relationship:
Workspace 1 → N Messages
User → Message
A user can send many messages.
Each message has one sender.
Relationship:
User 1 → N Messages
User → Notification
A user can receive many notifications.
Each notification belongs to one user.
Relationship:
User 1 → N Notifications
Workspace / Project → File
A workspace or project can contain multiple files.
Each file references the workspace and may optionally reference a project.
Relationship:
Workspace 1 → N Files
Project 1 → N Files
User → File
A user can upload many files.
Each file references the user who uploaded it.
Relationship:
User 1 → N Files
User → Task
A user can be assigned multiple tasks.
A task can have one assigned user in the current MVP design.
Relationship:
User 1 → N Tasks

5. Relationship Overview
User
 │
 ├── workspaceIds ──────────────┐
 │                              │
 ├── creates/owns ──────────► Workspace
 │                              │
 ├── sends ─────────────────► Message
 │                              │
 ├── receives ──────────────► Notification
 │                              │
 ├── uploads ───────────────► File
 │                              │
 └── assigned to ───────────► Task
                                │
                                ▼
                            Project
                                │
                                └── Tasks

A more complete logical structure is:

                    ┌─────────────┐
                    │    USER     │
                    └──────┬──────┘
                           │
                N          │          N
          ┌────────────────┴────────────────┐
          │                                 │
          ▼                                 ▼
   ┌─────────────┐                    ┌──────────────┐
   │  WORKSPACE  │                    │ NOTIFICATION │
   └──────┬──────┘                    └──────────────┘
          │
          │ 1:N
          ▼
   ┌─────────────┐
   │   PROJECT   │
   └──────┬──────┘
          │
          │ 1:N
          ▼
   ┌─────────────┐
   │    TASK     │
   └─────────────┘

WORKSPACE
   │
   ├──────────────► MESSAGE
   │                    │
   │                    └── sender → USER
   │
   └──────────────► FILE
                        │
                        └── uploadedBy → USER
6. Entity Relationship Summary
Entity	Relationship	Entity
User	N	Workspace
Workspace	1	Project
Project	1	Task
Workspace	1	Message
User	1	Message
User	1	Notification
Workspace	1	File
Project	1	File
User	1	File
User	1	Task
Project	N:1	Workspace
7. Access and Ownership Rules
Database relationships must support the following business rules:
A user must belong to a workspace before accessing workspace data.
A project must belong to a workspace.
A task must belong to a project.
A task can only be assigned to an authorized workspace member.
Files must belong to an authorized workspace or project.
Messages must belong to an authorized workspace context.
Notifications must belong to their intended user.
Protected resources must enforce authentication and authorization.
8. Data Integrity
The application layer must ensure that:
Referenced users exist before creating dependent records.
A project cannot reference an invalid workspace.
A task cannot reference an invalid project.
Task assignments reference valid workspace members.
Deleted or inaccessible resources cannot be accessed through unauthorized requests.
File metadata remains consistent with the associated workspace/project.
Notification ownership cannot be changed by unauthorized users.
9. Indexing Considerations
The following fields should be considered for database indexes:
Users.email
Users.workspaceIds
Workspaces.ownerId
Workspaces.memberIds
Projects.workspaceId
Tasks.projectId
Tasks.assignedTo
Messages.workspaceId
Messages.senderId
Files.workspaceId
Files.projectId
Notifications.userId
Notifications.isRead
Indexes should be added based on actual query patterns and performance testing.
10. ERD
The HiberTech database relationship model is represented by the following structure:

                         ┌─────────────┐
                         │    USERS    │
                         └──────┬──────┘
                                │
                         N      │      N
                                │
                         ┌──────▼──────┐
                         │ WORKSPACES  │
                         └──────┬──────┘
                                │
                    ┌───────────┼───────────┐
                    │           │           │
                   1:N         1:N         1:N
                    │           │           │
                    ▼           ▼           ▼
               PROJECTS     MESSAGES      FILES
                    │
                   1:N
                    │
                    ▼
                  TASKS

USERS
  │
  ├──────────────1:N────────────► NOTIFICATIONS
  │
  ├──────────────1:N────────────► MESSAGES
  │
  ├──────────────1:N────────────► FILES
  │
  └──────────────1:N────────────► TASKS
11. Current ERD Alignment
The current HiberTech ERD contains the following primary entities:
Users
Workspaces
Projects
Tasks
Messages
Files
Notifications
Audit Logs
The ERD also represents:
Workspace ownership
Workspace membership
Project ownership
Task assignment
Message replies
File uploads
User notifications
Optional audit logging
The current ERD is therefore aligned with the HiberTech MVP requirements.
12. Future Database Extensions
The database can later be extended for features outside the MVP, including:
Dedicated Invitations collection
Calendar events
Video meeting records
AI assistant conversations
Third-party integrations
Subscription and billing records
Advanced analytics
Mobile application data
These extensions should not be implemented as part of the initial HiberTech v1.0 MVP unless the project scope is formally changed.