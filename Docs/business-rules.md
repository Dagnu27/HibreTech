# HiberTech — Business Rules
## 1. Purpose
This document defines the core business rules that control workspace, project, task, file, communication, and access behavior within HiberTech.
These rules apply to the HiberTech MVP and provide consistent constraints for system design, development, and testing.
## 2. Workspace Rules
**BR-01:** A user must belong to a workspace before accessing workspace-specific data.
**BR-02:** Workspace owners or authorized administrators can invite members.
**BR-03:** Workspace membership determines a user's access to workspace data.
**BR-04:** A user must have appropriate permissions to perform administrative actions within a workspace.
**BR-05:** A removed workspace member must no longer have access to that workspace's protected resources.
## 3. Project Rules
**BR-06:** A project must belong to exactly one workspace.
**BR-07:** Project access must respect workspace membership and authorization rules.
**BR-08:** Only users with appropriate permissions can create, edit, or delete projects.
**BR-09:** Project members can only access project information they are authorized to view.
## 4. Task Rules
**BR-10:** A task must belong to exactly one project.
**BR-11:** A task may be assigned only to an authorized member of the associated workspace.
**BR-12:** Task status must follow the defined Kanban workflow:
**To Do → In Progress → Review → Done**
**BR-13:** Only authorized users can modify or delete tasks.
**BR-14:** A task may have a priority level and due date.
**BR-15:** Updating a task's status must persist the new status in the system.
## 5. Communication Rules
**BR-16:** Users must be authorized workspace members before accessing workspace-specific conversations.
**BR-17:** Users can send messages only in conversations they are authorized to access.
**BR-18:** Real-time messages must be associated with an authorized conversation or workspace context.
**BR-19:** Users may send supported emoji and image content through chat.
## 6. File Rules
**BR-20:** Files must be associated with an authorized workspace or project.
**BR-21:** Users may access files only when they have permission to access the associated workspace or project.
**BR-22:** File type and size must be validated before storage.
**BR-23:** Unauthorized users must not be able to download or preview protected files.
## 7. Notification Rules
**BR-24:** Notifications must belong to a specific user.
**BR-25:** Users can only view their own notifications.
**BR-26:** Relevant system events may generate notifications, including task assignments, new messages, and project updates.
**BR-27:** A notification can be marked as read by its intended user.
## 8. Security Rules
**BR-28:** Protected resources require authentication.
**BR-29:** Users can only access data permitted by their workspace membership and role.
**BR-30:** Authorization checks must be enforced on protected backend operations.
**BR-31:** Passwords must never be stored in plain text.
**BR-32:** Authentication credentials and sensitive security information must be handled securely.
**BR-33:** File uploads must be validated before being stored or made available to other users.
## 9. Data Integrity Rules
**BR-34:** A project cannot exist without a valid workspace association.
**BR-35:** A task cannot exist without a valid project association.
**BR-36:** A task assignment must reference a valid authorized workspace member.
**BR-37:** Deleted or inaccessible resources must not remain available through unauthorized direct requests.
## 10. MVP Rule Boundary
The business rules in this document apply to the HiberTech v1.0 MVP.
Advanced functionality such as AI assistance, video meetings, voice calls, calendar synchronization, third-party integrations, billing, payments, and advanced analytics is outside the current business-rule scope.
## 11. Traceability
The business rules are derived from:
* `docs/-vision.md`
* `docs/-requirements.md`
* `docs/-user-stories.md`
* `docs/-use-cases.md`
The documentation relationship is:
**Vision → Requirements → User Stories → Use Cases → Business Rules → Design → Development → Testing**
