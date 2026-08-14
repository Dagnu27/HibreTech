HiberTech — Use Cases
1. Purpose
This document describes the main interactions between HiberTech users and the system.
The use cases are derived from the functional requirements and user stories defined in:
docs/requirements.md
docs/user-stories.md
They provide a high-level view of how users interact with the HiberTech MVP.
2. Actors
Guest
An unauthenticated visitor who can access public pages and register or log in.
Member
A workspace member who participates in projects, tasks, chat, and file sharing.
Manager
A user responsible for managing projects, tasks, assignments, and team work.
Admin
A user with administrative responsibilities, including workspace access, member management, roles, and permissions.
3. Main Use Cases
UC-01 — Register
Primary Actor: Guest
Description:
The guest creates a HiberTech account using their name, email, and password.
Preconditions:
The user is not authenticated.
The registration page is accessible.
Main Flow:
Guest opens the registration page.
Guest enters their name, email, and password.
Guest submits the registration form.
System validates the submitted information.
System checks whether the email is already registered.
System securely stores the new account.
System confirms successful registration.
Alternative Flow:
If required information is invalid, the system displays validation errors.
If the email already exists, the system informs the guest that the account already exists.
Related Stories: US-01
UC-02 — Login
Primary Actor: Guest
Description:
The guest authenticates using their registered email and password.
Preconditions:
User has a registered account.
Main Flow:
Guest opens the login page.
Guest enters email and password.
System validates the credentials.
System creates an authenticated session/token.
System redirects the user to the authenticated application.
Alternative Flow:
Invalid credentials produce an appropriate error message.
An inactive or restricted account cannot access protected resources.
Related Stories: US-02
UC-03 — Reset Password
Primary Actor: Guest/User
Description:
A user resets their password after forgetting their current password.
Main Flow:
User selects the password reset option.
User provides the required account information.
System validates the reset request.
System provides a secure password reset mechanism.
User enters a new password.
System securely stores the new password.
System confirms the password reset.
Related Stories: US-04
UC-04 — Manage Profile
Primary Actor: Member / Manager / Admin
Description:
An authenticated user views and updates their profile information.
Main Flow:
User opens their profile.
System displays profile information.
User edits supported profile fields.
User optionally updates their profile photo.
User submits the changes.
System validates and saves the updated information.
Related Stories: US-05, US-06, US-07
UC-05 — Create Workspace
Primary Actor: Member
Description:
A user creates a workspace for team collaboration.
Main Flow:
User selects create workspace.
User enters workspace information.
System validates the information.
System creates the workspace.
User becomes the workspace owner/admin.
Workspace appears in the user's workspace list.
Related Stories: US-08
UC-06 — Invite Member
Primary Actor: Manager / Admin
Description:
An authorized user invites another user to join a workspace.
Main Flow:
Manager/Admin opens workspace member management.
User enters the invitee's email.
System creates an invitation.
Invitation is sent or made available to the invitee.
Invitee accepts the invitation.
System adds the invitee to the workspace.
Alternative Flow:
Invalid or expired invitations are rejected.
An existing workspace member cannot be invited again.
Related Stories: US-09, US-10
UC-07 — Manage Workspace Members
Primary Actor: Admin / Manager
Description:
An authorized user views and manages workspace membership.
Main Flow:
Admin/Manager opens workspace members.
System displays authorized workspace members.
Admin/Manager selects a member.
User performs an allowed management action.
System validates permissions.
System saves the membership change.
Possible Actions:
View members
Remove members
Manage supported access permissions
Related Stories: US-11, US-12, US-43, US-44
UC-08 — Create Project
Primary Actor: Manager / Admin
Description:
An authorized user creates a project inside a workspace.
Main Flow:
User selects a workspace.
User selects create project.
User enters project information.
System validates the information.
System creates the project.
Project becomes available to authorized workspace members.
Related Stories: US-17
UC-09 — Manage Project
Primary Actor: Manager / Admin
Description:
An authorized user views, edits, or deletes a project.
Main Flow:
User opens a project.
System displays project details.
User selects an allowed action.
User edits or deletes the project.
System validates the user's permissions.
System saves or applies the requested change.
Possible Actions:
View project details
Edit project
Delete project
View project tasks
Related Stories: US-18, US-19, US-20
UC-10 — Create Task
Primary Actor: Manager / Member
Description:
An authorized user creates a task within a project.
Main Flow:
User opens a project.
User selects create task.
User enters task information.
System validates the information.
System creates the task.
Task appears in the project's task list and Kanban board.
Related Stories: US-21
UC-11 — Assign Task
Primary Actor: Manager
Description:
A manager assigns a task to a workspace member.
Main Flow:
Manager opens a task.
Manager selects a workspace member.
Manager assigns the task.
System validates the assignment.
System saves the assignment.
Assigned member can view the task.
System may generate a task notification.
Related Stories: US-22, US-39
UC-12 — Update Task
Primary Actor: Manager / Member
Description:
An authorized user updates task information and progress.
Main Flow:
User opens a task.
User changes supported task information.
System validates the change.
System saves the updated task.
Updated information is displayed to authorized users.
Possible Actions:
Update task status
Set priority
Set due date
Update task details
Delete task when authorized
Related Stories: US-23, US-24, US-25, US-26
UC-13 — Move Task on Kanban
Primary Actor: Manager / Member
Description:
A user moves a task between Kanban columns to update its workflow status.
Main Flow:
User opens the project Kanban board.
System displays tasks in their current columns.
User drags or moves a task to another column.
System updates the task status.
System persists the change.
Board reflects the new task position.
Alternative Flow:
If saving fails, the system informs the user and restores or retains the previous valid state.
Related Stories: US-27, US-28, US-29
UC-14 — Send Message
Primary Actor: Member / Manager / Admin
Description:
A workspace user communicates with other authorized users through real-time chat.
Main Flow:
User opens a permitted conversation.
User enters a message.
User sends the message.
System validates and stores the message.
Message is delivered to authorized participants.
Participants see the message in real time.
Supported Content:
Text
Emoji
Supported images
Related Stories: US-30, US-31, US-32, US-33, US-34
UC-15 — Upload File
Primary Actor: Member / Manager / Admin
Description:
A user uploads a file to a permitted workspace or project.
Main Flow:
User opens a workspace or project.
User selects file upload.
User selects a file.
System validates file type and size.
System uploads the file to storage.
System associates the file with the selected workspace or project.
Authorized users can access the file.
Related Stories: US-35, US-38
UC-16 — Download File
Primary Actor: Member / Manager / Admin
Description:
An authorized user downloads a shared file.
Main Flow:
User opens an accessible workspace or project.
User selects a shared file.
System verifies access permissions.
System retrieves the file.
File download begins.
Alternative Flow:
Unauthorized users are denied access.
Missing files produce an appropriate error message.
Related Stories: US-36, US-37, US-38
UC-17 — View Notifications
Primary Actor: Member / Manager / Admin
Description:
A user views notifications related to tasks, messages, and project activity.
Main Flow:
User opens the notification area.
System retrieves notifications belonging to the user.
System displays unread and read notifications.
User selects a notification.
System opens the related resource when available.
Notification Examples:
New task assignment
New message
Project update
Related Stories: US-39, US-40, US-41
UC-18 — Manage Notifications
Primary Actor: Member / Manager / Admin
Description:
A user manages the read state of their notifications.
Main Flow:
User opens notifications.
User selects a notification.
User marks the notification as read.
System updates the notification state.
Updated state is displayed.
Related Stories: US-42
UC-19 — Manage Users
Primary Actor: Admin
Description:
An administrator manages users and access within the system or workspace.
Main Flow:
Admin opens user management.
System displays users the admin is authorized to manage.
Admin selects a user.
Admin performs an authorized management action.
System validates permissions.
System saves the change.
Possible Actions:
View users
Manage workspace membership
Remove users from a workspace
Manage supported roles and permissions
Related Stories: US-12, US-43, US-44
UC-20 — View System Statistics
Primary Actor: Admin
Description:
An administrator views available system-level statistics.
MVP Note:
This use case is reserved for future versions because advanced analytics and system statistics are outside the HiberTech v1.0 MVP scope.
Future Examples:
Total users
Total workspaces
Total projects
Task activity
Workspace activity
Status: Future / Out of Scope for v1.0
4. Use Case Relationships
The main HiberTech workflow can be represented as:
Register/Login → Workspace → Project → Task → Kanban → Chat/Files → Notifications
Some use cases depend on successful authentication.
For example:
UC-05 Create Workspace requires authentication.
UC-08 Create Project requires workspace membership and appropriate permissions.
UC-10 Create Task requires access to a project.
UC-14 Send Message requires access to the relevant conversation.
UC-15 Upload File requires permission to the target workspace or project.
5. Traceability
Use Case	Related User Stories
UC-01 Register	US-01
UC-02 Login	US-02
UC-03 Reset Password	US-04
UC-04 Manage Profile	US-05–US-07
UC-05 Create Workspace	US-08
UC-06 Invite Member	US-09–US-10
UC-07 Manage Workspace Members	US-11–US-12, US-43–US-44
UC-08 Create Project	US-17
UC-09 Manage Project	US-18–US-20
UC-10 Create Task	US-21
UC-11 Assign Task	US-22, US-39
UC-12 Update Task	US-23–US-26
UC-13 Move Task on Kanban	US-27–US-29
UC-14 Send Message	US-30–US-34
UC-15 Upload File	US-35, US-38
UC-16 Download File	US-36–US-38
UC-17 View Notifications	US-39–US-41
UC-18 Manage Notifications	US-42
UC-19 Manage Users	US-12, US-43–US-44
UC-20 View System Statistics	Future / Out of Scope
6. MVP Boundary
The HiberTech v1.0 MVP focuses on:
Authentication → Workspace Management → Project Management → Task Management → Kanban → Real-Time Chat → File Sharing → Notifications
The following remain outside the MVP:
AI assistant
Video meetings
Voice calls
Calendar synchronization
Mobile applications
Third-party integrations
Billing and payments
Advanced analytics
System statistics
Use cases outside the MVP may be documented as future requirements when the product expands beyond the initial release.




