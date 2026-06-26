# Project Management System — Complete PRD & Build Guide

---

## PRODUCT REQUIREMENTS DOCUMENT (PRD)

### Project Name
Project Management System

### What This App Does
A web application for one organization with three teams. The Admin manages everything from a central dashboard. Each team has one Team Head and optional Team Members. Each team tracks different data (Training, Social Media, or Progress). The Admin builds custom columns for each team's data table. Tasks are created by Team Heads and assigned to members. Users are invited by email only — no open registration.

---

### Accounts in This System

| Role | Email |
|------|-------|
| Admin | gowri.a@hicas.ac.in |
| Team 1 Head | gowritrendline@gmail.com |
| Team 2 Head | gowrihindusthan@gmail.com |
| Team 3 Head | gowricloud@gmail.com |

---

### Roles Summary

| Role | What They Can Do |
|------|----------------|
| Admin | Full read-only view of all teams, manage users/teams/columns, send invitations, comment on records, view reports, filter all data |
| Team Head | Full CRUD on their own team's records, manage tasks, invite and manage team members, view notifications |
| Member | Add and edit records in their team, update status of their assigned tasks, view notifications |

---

## PAGES — COMPLETE LIST (Based on All Requirements)

---

### PUBLIC PAGES (No login needed)

| Page | File | Description |
|------|------|-------------|
| Login | `/login` | Email + password login. Redirects to correct dashboard by role after login. Session expired message shown if token expired. |
| Forgot Password | `/forgot-password` | Enter email. System sends reset link email. |
| Reset Password | `/reset-password?token=` | Enter new password and confirm. Token expires in 1 hour. Single use only. |
| Accept Invitation | `/accept-invitation?token=` | New user sets their full name and password from invite email link. Token expires in 48 hours. Single use only. |

---

### ADMIN PAGES

| Page | Path | Description |
|------|------|-------------|
| Admin Dashboard | `/admin/dashboard` | Overview cards: Total Teams, Total Users, Total Records across all teams, Total Tasks. Summary bar for each team showing completion %. Real-time updates via Socket.io. Notification bell in navbar. |
| User Management | `/admin/users` | Table of all users (Admin, Team Heads, Members). Columns: Name, Email, Role, Team, Status (Active/Inactive), Date Joined. Invite User button opens modal. Each row has Deactivate button. Admin can view but NOT add/remove team members — only view. |
| Invite User Modal | (modal on `/admin/users`) | Fields: Full Name, Email, Role dropdown (Team Head, Member), Team dropdown (Team 1, Team 2, Team 3). Send Invite button. Shows pending/accepted/expired status per invitation. Resend button for expired invites. |
| Invitation Tracker | `/admin/invitations` | Table of all sent invitations: Email, Role, Team, Status (Pending / Accepted / Expired), Sent Date. Resend button for expired ones. |
| Team Management | `/admin/teams` | Three team cards. Each card shows: Team Name, Team Head name, number of members, team type. Click to expand and view members list (read-only). |
| Team Data View — Team 1 | `/admin/teams/1/records` | Read-only table of Team 1 (Training Tracker) records with dynamic columns. Filter by status (Completed / Not Completed) and date range. Admin can add comments on any record. Search bar to filter records by keyword. Export to CSV button. |
| Team Data View — Team 2 | `/admin/teams/2/records` | Read-only table of Team 2 (Social Media Tracker) records with dynamic columns. Filter by status (Posted / Not Posted Yet), platform, and date range. Admin can comment, search, and export. |
| Team Data View — Team 3 | `/admin/teams/3/records` | Read-only table of Team 3 (Progress Tracker) records with dynamic columns. Filter by status (On Track / In Progress / Delayed / Completed) and due date range. Admin can comment, search, and export. |
| Record Detail (Admin view) | `/admin/teams/:teamId/records/:recordId` | Full view of a single record. Shows all column data, attachments, comment thread (Admin comments labeled separately), status change history, and record edit history (audit trail). |
| Column Builder | `/admin/columns` | Select a team from dropdown. Shows that team's current columns in a drag-reorderable list. Each column row shows: column name, type, required badge, drag handle, edit icon, delete icon. Add New Column button opens a modal. Column types available: Text, Number, Date, Date and Time, URL, Select, Multi-Select, Assignee, Image/File Upload, Status, Checkbox, Email, Phone Number. Admin can mark any column as Required. |
| Task View (All Teams) | `/admin/tasks` | Read-only Kanban board showing tasks across all three teams. Filter by team, status, priority, due date, and assigned person. Overdue tasks highlighted. |
| Task Detail (Admin view) | `/admin/tasks/:taskId` | Read-only view of a task. Shows: title, description, assignee, due date, priority, status, subtasks, comments. |
| Reports | `/admin/reports` | Summary report across all teams. Filter by time period. Cards: Total Records, Completion Rate per Team, Overdue Items, Task Progress. Table with per-team breakdown. Export button. Activity logs across all teams (who did what and when). |
| Notifications | (bell icon dropdown in navbar) | All notifications for Admin: Team Head added a record, status updated, task completed, team targets hit. Shows: what happened, who did it, which team, which record, time ago. Mark as Read / Mark All as Read. |
| Profile Settings | `/admin/profile` | Fields: Full Name, Email (read-only), Current Password, New Password, Confirm New Password. Save button. |

---

### TEAM HEAD PAGES

| Page | Path | Description |
|------|------|-------------|
| Team Head Dashboard | `/team/dashboard` | Shows their team name, member count, progress bar (tasks completed vs total), summary cards: Total Records, Completed, Pending, Overdue. Notification bell in navbar. Real-time updates. |
| Team Records Table | `/team/records` | The team's data table built from Admin's dynamic columns. Columns are auto-generated from column builder. Features: Add Record button, Edit Record, Delete Record (Team Head only), search bar, column visibility toggle (show/hide columns — personal preference only), pin important records to top. Bulk select records and bulk update status. Overdue records highlighted. |
| Add/Edit Record Modal | (modal on `/team/records`) | Form fields auto-generated from the team's dynamic column config. Required fields enforced. Assignee field pulls from team members list. File/image upload handled inline. |
| Record Detail | `/team/records/:recordId` | Full view of one record. Shows all column data, attachments (view + upload more), comment thread (internal team discussion + Admin comments labeled separately), status change history log, record edit history / audit trail. |
| Task Board | `/team/tasks` | Kanban board for this team's tasks only. Columns: To Do, In Progress, Done. Each card shows: title, assignee, due date, priority badge, overdue highlight. Create Task button opens modal. Drag-and-drop cards between columns. |
| Add/Edit Task Modal | (modal on `/team/tasks`) | Fields: Title, Description, Assignee dropdown (from team members), Due Date, Priority (High / Medium / Low), Status. Also shows subtasks section: add subtasks inline, each with same fields. |
| Task Detail | `/team/tasks/:taskId` | Full task view: title, description, assignee, due date, priority, status. Subtasks listed below with checkboxes. When all subtasks done → main task auto-marked Done. Comments section. Edit and Delete buttons (Team Head only). Reassign task to another member option. |
| Team Members | `/team/members` | Table of all members in this team. Columns: Name, Email, Date Joined, Records Added/Updated (contribution count). Invite Member button opens email invite modal. Remove Member button per row. |
| Invite Member Modal | (modal on `/team/members`) | Fields: Email. Send Invite button. Invitation tracked (pending/accepted/expired). |
| Notifications | (bell icon dropdown in navbar) | Notifications for Team Head: Admin commented on a record, task due tomorrow (their team), member completed a task, new member joined, Admin left feedback. Mark as Read / Mark All as Read. |
| Activity Log | `/team/activity` | Full activity log for this team only. Every action: record added/edited/deleted, task created/updated, member joined/removed, status changes — with who did it and when. |
| Profile Settings | `/team/profile` | Same as Admin's profile settings page. Full Name, Email (read-only), password change. |

---

### MEMBER PAGES

| Page | Path | Description |
|------|------|-------------|
| Member Dashboard | `/member/dashboard` | Shows team name, their assigned tasks summary: Total, Done, In Progress, Overdue. Notification bell. |
| Team Records Table | `/member/records` | Same table as Team Head's records view but Member can: Add Record, Edit Record. Cannot Delete records. Cannot pin or bulk-action. Column visibility toggle available. |
| Add/Edit Record Modal | (modal on `/member/records`) | Same auto-generated form as Team Head's. Required fields enforced. |
| Record Detail | `/member/records/:recordId` | Full record view. Can see and join internal comment thread. Can upload additional attachments. Can view status change history. |
| My Tasks | `/member/tasks` | List of tasks assigned to this member only. Shows: title, team, due date, priority badge, status. Overdue items highlighted. Member can update status of their own tasks. |
| Task Detail | `/member/tasks/:taskId` | Full task view. Member can update their task status and add comments. Cannot delete or reassign. |
| Notifications | (bell icon dropdown in navbar) | Notifications for Member: new task assigned, task due tomorrow reminder. Mark as Read / Mark All as Read. |
| Profile Settings | `/member/profile` | Full Name, Email (read-only), password change. |

---

### SHARED / SYSTEM PAGES

| Page | Path | Description |
|------|------|-------------|
| 404 Not Found | `/404` | "Page Not Found" message with button to go back to dashboard. |
| Session Expired | (redirect from any protected page) | When JWT token expires, user is automatically redirected to `/login` with a clear message: "Your session has expired. Please log in again." No broken page shown. |

---

## DATABASE TABLES

| Table | Key Columns |
|-------|-------------|
| users | id, full_name, email, password_hash, role (admin/team_head/member), team_id, is_active, created_at |
| teams | id, name, type (training/social_media/progress), head_user_id, created_at |
| team_columns | id, team_id, column_name, column_type, is_required, order_index, created_at |
| records | id, team_id, created_by, is_pinned, created_at, updated_at |
| record_values | id, record_id, column_id, value_text, value_number, value_date, value_json |
| record_history | id, record_id, column_id, old_value, new_value, changed_by, changed_at |
| record_attachments | id, record_id, file_url, file_name, uploaded_by, uploaded_at |
| tasks | id, title, description, assignee_id, created_by, team_id, parent_task_id (null = main task), priority, status, due_date, created_at |
| comments | id, record_id (nullable), task_id (nullable), user_id, content, is_admin_comment, created_at |
| status_change_log | id, record_id (nullable), task_id (nullable), old_status, new_status, changed_by, changed_at |
| invitations | id, email, role, team_id, token, accepted, expires_at, created_at |
| notifications | id, user_id, message, type, related_team_id, related_record_id, related_task_id, is_read, created_at |
| activity_log | id, team_id, user_id, action, details, created_at |
| password_reset_tokens | id, user_id, token, expires_at, used, created_at |

---

## TECH STACK

| Layer | Technology |
|-------|-----------|
| Frontend | React + Vite + Tailwind CSS + Redux Toolkit + React Router DOM |
| Backend | Node.js + Express.js |
| Database | PostgreSQL via Supabase (free tier) |
| Auth | JWT (JSON Web Tokens) + bcrypt |
| Real-time | Socket.io (frontend + backend) |
| Email | Nodemailer with Gmail SMTP or Resend |
| Frontend Hosting | Vercel (free) |
| Backend Hosting | Render.com (free tier) |
| Version Control | GitHub |

---

## ENVIRONMENT VARIABLES

### Backend `.env`
```
DATABASE_URL=
JWT_SECRET=
JWT_EXPIRES_IN=7d
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=
EMAIL_PASS=
PORT=5000
FRONTEND_URL=
SUPABASE_URL=
SUPABASE_SERVICE_ROLE_KEY=
```

### Frontend `.env`
```
VITE_API_URL=
VITE_FRONTEND_URL=
```

---

## UI & DESIGN RULES

- Light theme only. No dark mode.
- Clean, minimal design like Notion — white backgrounds, simple borders, clean typography.
- Admin layout: top navbar (with notification bell + user name + logout) + left sidebar + main content area.
- Team Head layout: top navbar (with notification bell + user name + logout) + left sidebar showing ONLY their team's sections.
- Member layout: top navbar + left sidebar with limited links.
- Sidebar collapses on mobile.
- Tables scroll horizontally on mobile.
- Modals go full screen on mobile.
- Fully responsive on all screen sizes.

---

## SECURITY RULES

- All passwords hashed with bcrypt. No plain text ever stored.
- Every API request requires a valid JWT in the Authorization header.
- Invitation tokens and reset tokens are single-use and expire.
- Role checks enforced on every API endpoint — wrong role = rejected.
- Multi-tenant isolation enforced at database level. Team Head can never fetch another team's data even by URL manipulation.
- No page accessible without login. Any direct URL access without valid token → redirected to login.

---

## 50-PHASE BUILD PLAN

---

### PHASE 1 — Create All Accounts

- Go to github.com — create a free account
- Go to vercel.com — sign up using your GitHub account
- Go to supabase.com — create a free account
- Go to render.com — create a free account
- Go to resend.com — create a free account (for emails)
- Save all account emails and passwords in a Google Doc or Notion page right now
- Do not move to Phase 2 until all accounts are created

---

### PHASE 2 — Create Tracker Document

- Open Google Docs or Notion
- Create a new document named: Project Management System — Build Tracker
- Add these sections: Credentials, API Keys, URLs, Phase Checklist (all 50 phases)
- Bookmark the document in your browser

---

### PHASE 3 — Create GitHub Repository and Set Up Project Folder

- Go to github.com → New Repository → name it: project-management-system → Private
- Copy the repository URL
- In terminal: `git clone YOUR_REPO_URL`
- Open the cloned folder in Antigravity

---

### PHASE 4 — Create the React Frontend Project

- Inside project folder in terminal:
- `mkdir frontend && cd frontend`
- `npm create vite@latest . -- --template react`
- `npm install`
- `npm install tailwindcss @tailwindcss/vite react-router-dom axios @reduxjs/toolkit react-redux socket.io-client`
- Ask Antigravity AI: "Set up Tailwind CSS v4 in this Vite React project"
- Run `npm run dev` to confirm it runs at localhost:5173

---

### PHASE 5 — Set Up Frontend Folder Structure

- Ask Antigravity AI:
  - "Inside the src folder create these folders: pages/admin, pages/team, pages/member, pages/public, components, layouts, context, hooks, utils, api, store. Create an index.js inside each folder."

---

### PHASE 6 — Build Login Page UI

- Ask Antigravity AI:
  - "Build src/pages/public/Login.jsx — email input, password input, Forgot Password link, Login button. Clean minimal Notion-style design with Tailwind CSS. Show a 'Session expired' banner when redirected from an expired token."

---

### PHASE 7 — Build Forgot Password and Reset Password Pages UI

- Ask Antigravity AI:
  - "Build src/pages/public/ForgotPassword.jsx — email input + Send Reset Link button."
  - "Build src/pages/public/ResetPassword.jsx — new password + confirm password + Reset button. Read token from URL query param."

---

### PHASE 8 — Build Accept Invitation Page UI

- Ask Antigravity AI:
  - "Build src/pages/public/AcceptInvitation.jsx — Full Name input, Email (read-only), Create Password, Confirm Password. Button: Activate My Account. Read token from URL."

---

### PHASE 9 — Build Sidebar and TopBar Layout Components

- Ask Antigravity AI:
  - "Build src/layouts/Sidebar.jsx — navigation links that change based on user role (admin/team_head/member). Collapsible on mobile. Clean minimal style."
  - "Build src/layouts/TopBar.jsx — left: app logo/name. Right: notification bell icon (with unread count badge), user name, logout button."

---

### PHASE 10 — Build Dashboard Layout Wrapper

- Ask Antigravity AI:
  - "Build src/layouts/DashboardLayout.jsx — Sidebar on left, TopBar at top, main content area on right. Wrap all protected pages with this."

---

### PHASE 11 — Build Admin Dashboard Page UI

- Ask Antigravity AI:
  - "Build src/pages/admin/AdminDashboard.jsx — Summary cards: Total Teams (3), Total Users, Total Records, Total Tasks. Below: three team progress bars showing completion %. Use DashboardLayout. Placeholder data for now."

---

### PHASE 12 — Build Team Head Dashboard Page UI

- Ask Antigravity AI:
  - "Build src/pages/team/TeamDashboard.jsx — Show team name, member count, progress bar (tasks done vs total), cards: Total Records, Completed, Pending, Overdue. Use DashboardLayout."

---

### PHASE 13 — Build Member Dashboard Page UI

- Ask Antigravity AI:
  - "Build src/pages/member/MemberDashboard.jsx — Show team name, my tasks summary: Total, Done, In Progress, Overdue. Use DashboardLayout."

---

### PHASE 14 — Build User Management Page UI (Admin)

- Ask Antigravity AI:
  - "Build src/pages/admin/UserManagement.jsx — Table: Full Name, Email, Role, Team, Status, Joined Date. Invite User button at top. Deactivate button per row. DashboardLayout. Dummy data."

---

### PHASE 15 — Build Invite User Modal UI

- Ask Antigravity AI:
  - "Build src/components/InviteUserModal.jsx — Fields: Full Name, Email, Role dropdown (Team Head, Member), Team dropdown (Team 1, Team 2, Team 3). Send Invite and Cancel buttons."

---

### PHASE 16 — Build Invitation Tracker Page UI (Admin)

- Ask Antigravity AI:
  - "Build src/pages/admin/InvitationTracker.jsx — Table: Email, Role, Team, Status (Pending/Accepted/Expired), Sent Date. Resend button for expired rows. DashboardLayout."

---

### PHASE 17 — Build Team Management Page UI (Admin)

- Ask Antigravity AI:
  - "Build src/pages/admin/TeamManagement.jsx — Three team cards. Each shows: team name, team type, team head name, member count. Click to expand and see members list (read-only). DashboardLayout."

---

### PHASE 18 — Build Admin Team Data View Pages (3 pages)

- Ask Antigravity AI:
  - "Build src/pages/admin/TeamRecords.jsx — A read-only data table that dynamically renders columns based on the team selected. Shows records from the selected team. Filter bar: status dropdown, date range picker, platform (for Team 2). Search bar. Export CSV button. Comment icon per row. DashboardLayout."

---

### PHASE 19 — Build Record Detail Page UI

- Ask Antigravity AI:
  - "Build src/pages/admin/RecordDetail.jsx — Full record view: all dynamic column data, file attachments section (view only for Admin), comment thread (Admin comments labeled), status change history log, record edit audit trail. DashboardLayout."

---

### PHASE 20 — Build Column Builder Page UI (Admin)

- Ask Antigravity AI:
  - "Build src/pages/admin/ColumnBuilder.jsx — Team selector dropdown at top. Below: drag-reorderable list of columns for that team. Each row: drag handle, column name, type badge, Required badge (if set), edit icon, delete icon. Add New Column button. DashboardLayout."

---

### PHASE 21 — Build Add/Edit Column Modal UI

- Ask Antigravity AI:
  - "Build src/components/ColumnModal.jsx — Fields: Column Name (text input), Column Type dropdown (Text, Number, Date, Date and Time, URL, Select, Multi-Select, Assignee, Image/File Upload, Status, Checkbox, Email, Phone Number), Required toggle. For Select and Multi-Select types show an input to add option labels. Save and Cancel buttons."

---

### PHASE 22 — Build Task Board Page UI

- Ask Antigravity AI:
  - "Build src/pages/team/TaskBoard.jsx — Kanban board with three columns: To Do, In Progress, Done. Task cards show: title, assignee name, due date, priority badge (color coded), overdue highlight (red border). Create Task button. Admin version (src/pages/admin/TaskView.jsx) is same but read-only with team filter dropdown. DashboardLayout."

---

### PHASE 23 — Build Add/Edit Task Modal UI

- Ask Antigravity AI:
  - "Build src/components/TaskModal.jsx — Fields: Title, Description, Assignee dropdown (team members), Due Date, Priority (High/Medium/Low), Status. Subtasks section: list of subtasks each with title, assignee, due date, priority, status. Add Subtask button. Save and Cancel."

---

### PHASE 24 — Build Task Detail Page UI

- Ask Antigravity AI:
  - "Build src/pages/team/TaskDetail.jsx — Shows: title, description, assignee, due date, priority, status, created by, created at. Edit and Delete buttons (Team Head only). Subtasks checklist. When all subtasks checked → show 'All done!' and auto-update main task status indicator. Comments section below. Reassign button. DashboardLayout."

---

### PHASE 25 — Build Team Records Table Page UI (Team Head & Member)

- Ask Antigravity AI:
  - "Build src/pages/team/TeamRecords.jsx — Dynamic data table. Columns auto-generated from this team's column config. Add Record button (Team Head and Member). Delete Record button visible only to Team Head. Search bar. Column visibility toggle (checkboxes per column — personal display only). Pin icon per row (Team Head only). Bulk select checkboxes + Bulk Status Update button (Team Head only). Overdue rows highlighted. DashboardLayout."

---

### PHASE 26 — Build Add/Edit Record Modal UI (Dynamic Form)

- Ask Antigravity AI:
  - "Build src/components/RecordModal.jsx — Dynamically renders form fields based on team's column config fetched from API. Each field type renders correctly: text input, number input, date picker, datetime picker, URL input, single select dropdown, multi-select tags, assignee dropdown (from team members), file/image upload, status dropdown with custom labels, checkbox, email input, phone input. Required fields show asterisk and block save if empty."

---

### PHASE 27 — Build Record Detail Page UI (Team View)

- Ask Antigravity AI:
  - "Build src/pages/team/RecordDetail.jsx — Full record view for Team Head and Member. Shows all column data, file attachments (with upload more button), internal comment thread (Team Head and Member can comment, Admin comments labeled), status change history, edit audit trail. DashboardLayout."

---

### PHASE 28 — Build Team Members Page UI (Team Head)

- Ask Antigravity AI:
  - "Build src/pages/team/TeamMembers.jsx — Table: Name, Email, Date Joined, Records Contributed (count). Invite Member button opens email invite modal. Remove Member button per row (Team Head only). DashboardLayout."

---

### PHASE 29 — Build Activity Log Page UI

- Ask Antigravity AI:
  - "Build src/pages/team/ActivityLog.jsx — Chronological list of all actions in this team: record added, record edited, task created, member joined, status changed. Each entry shows: action description, who did it, time ago. Admin version at src/pages/admin/ActivityLog.jsx shows all teams with team filter. DashboardLayout."

---

### PHASE 30 — Build Reports Page UI (Admin)

- Ask Antigravity AI:
  - "Build src/pages/admin/Reports.jsx — Time period filter (date range picker). Summary cards: Total Records across all teams, Overall Completion Rate, Overdue Items, Task Progress. Below: per-team breakdown table: Team Name, Total Records, Completed, Pending, Overdue, Completion %. Export to CSV button. DashboardLayout."

---

### PHASE 31 — Build My Tasks Page UI (Member)

- Ask Antigravity AI:
  - "Build src/pages/member/MyTasks.jsx — List of tasks assigned to logged-in member. Columns: Task Title, Due Date, Priority badge, Status dropdown (member can update their own task status). Overdue items highlighted in red. Clicking a task goes to TaskDetail page. DashboardLayout."

---

### PHASE 32 — Build Profile Settings Page UI

- Ask Antigravity AI:
  - "Build src/pages/shared/ProfileSettings.jsx — Full Name input, Email (read-only), Current Password, New Password, Confirm New Password. Save Changes button. Used by all three roles."

---

### PHASE 33 — Build Notifications Dropdown UI

- Ask Antigravity AI:
  - "Build src/components/NotificationsDropdown.jsx — Bell icon in TopBar. On click shows dropdown list of notifications. Each item: icon, message text (what happened, who, which team/record, time ago). Mark as Read button per item. Mark All as Read button at top. Unread count badge on bell. Link to related record or task on click."

---

### PHASE 34 — Build 404 Page and Session Expired Handling

- Ask Antigravity AI:
  - "Build src/pages/public/NotFound.jsx — 'Page Not Found' message + Go to Dashboard button."
  - "In the axios interceptor, when a 401 response is received, clear localStorage, set a sessionExpired flag, and redirect to /login. On the Login page show a banner: 'Your session has expired. Please log in again.' only when this flag is set."

---

### PHASE 35 — Set Up React Router with Role-Based Protection

- Ask Antigravity AI:
  - "Set up React Router in src/App.jsx. Public routes: /login, /forgot-password, /reset-password, /accept-invitation. Protected admin routes (admin role only): /admin/dashboard, /admin/users, /admin/invitations, /admin/teams, /admin/teams/:teamId/records, /admin/teams/:teamId/records/:recordId, /admin/tasks, /admin/tasks/:taskId, /admin/columns, /admin/reports, /admin/activity, /admin/profile. Protected team_head routes: /team/dashboard, /team/records, /team/records/:recordId, /team/tasks, /team/tasks/:taskId, /team/members, /team/activity, /team/profile. Protected member routes: /member/dashboard, /member/records, /member/records/:recordId, /member/tasks, /member/tasks/:taskId, /member/profile. 404 catch-all."

---

### PHASE 36 — Add Auth Context and Protected Route

- Ask Antigravity AI:
  - "Build src/context/AuthContext.jsx — stores user object and JWT token in localStorage. Provides: login(), logout(), useAuth hook. Wrap app in main.jsx."
  - "Build src/components/ProtectedRoute.jsx — redirects to /login if not authenticated. Accepts allowedRoles prop. Redirects to /404 if wrong role."
  - "Apply role-based sidebar links: Admin sees all admin links, Team Head sees team links, Member sees member links only."

---

### PHASE 37 — Create Supabase Project and Get Credentials

- Go to supabase.com → New Project → name: project-management-system
- Save DB password in tracker document
- Go to Settings → API → copy: Project URL, anon key, service role key
- Go to Settings → Database → copy Connection String (URI format)
- Save all in tracker document

---

### PHASE 38 — Create All Database Tables in Supabase

- Ask Antigravity AI: "Write complete SQL to create all these tables in PostgreSQL: users, teams, team_columns, records, record_values, record_history, record_attachments, tasks, comments, status_change_log, invitations, notifications, activity_log, password_reset_tokens — with all columns from the PRD"
- Paste into Supabase SQL Editor → Run
- Verify all 14 tables exist in Table Editor

---

### PHASE 39 — Insert Seed Data

- Ask Antigravity AI: "Write SQL to insert: 3 teams (Team 1 Training Tracker, Team 2 Social Media Tracker, Team 3 Progress Tracker) and the Admin user (gowri.a@hicas.ac.in, role admin, hashed password). Also insert default Kanban status column values: To Do, In Progress, Done."
- Run in Supabase SQL Editor

---

### PHASE 40 — Set Up Row Level Security

- Ask Antigravity AI: "Write Supabase RLS policies for all 14 tables so: Admin sees and does everything, Team Head sees only their team's data, Member sees only their team's data and only their assigned tasks. All isolation enforced at DB level."
- Run in Supabase SQL Editor

---

### PHASE 41 — Create Backend Project

- In terminal at project root:
- `mkdir backend && cd backend`
- `npm init -y`
- `npm install express cors dotenv bcryptjs jsonwebtoken nodemailer @supabase/supabase-js socket.io`
- `npm install -D nodemon`
- Add to package.json scripts: `"dev": "nodemon server.js"`

---

### PHASE 42 — Set Up Backend Folder Structure and server.js

- Ask Antigravity AI:
  - "Create folders in backend/: routes/, controllers/, middleware/, utils/, config/. Create server.js that sets up Express, CORS, JSON parsing, Socket.io, and loads all routes. Use dotenv."

---

### PHASE 43 — Create Backend .env File

- Create backend/.env with all variables from PRD section
- Create backend/.gitignore and add .env to it

---

### PHASE 44 — Build Auth API

- Ask Antigravity AI:
  - "Build backend/routes/auth.js and backend/controllers/authController.js for: POST /api/auth/login (verify email+password, return JWT + user object with role), POST /api/auth/forgot-password (create reset token, send email), POST /api/auth/reset-password (verify token, update password, invalidate token), POST /api/auth/accept-invitation (verify invite token, set fullName + password, activate user)"

---

### PHASE 45 — Build JWT Middleware and Role Guard

- Ask Antigravity AI:
  - "Build backend/middleware/auth.js — verifyToken middleware (reads Bearer token, verifies JWT, attaches req.user). Build requireRole middleware that accepts array of allowed roles and rejects if user role not in list."

---

### PHASE 46 — Build User and Invitation APIs

- Ask Antigravity AI:
  - "Build backend/routes/users.js and userController.js for: GET /api/users (Admin), POST /api/users/invite (Admin — create invitation + send email), PUT /api/users/:id/deactivate (Admin), GET /api/users/me, PUT /api/users/me"
  - "Build backend/routes/invitations.js — GET /api/invitations (Admin — list all with status), POST /api/invitations/resend/:id (Admin)"

---

### PHASE 47 — Build Team and Column Builder APIs

- Ask Antigravity AI:
  - "Build backend/routes/teams.js and teamController.js for: GET /api/teams (Admin), GET /api/teams/:id (Admin + Team Head), GET /api/teams/:id/members (Admin + Team Head), POST /api/teams/:id/invite-member (Team Head), DELETE /api/teams/:id/members/:userId (Team Head)"
  - "Build backend/routes/columns.js and columnController.js for: GET /api/columns/:teamId, POST /api/columns (Admin), PUT /api/columns/:id (Admin), DELETE /api/columns/:id (Admin), PUT /api/columns/reorder (Admin — accepts ordered array of IDs)"

---

### PHASE 48 — Build Records API (Dynamic)

- Ask Antigravity AI:
  - "Build backend/routes/records.js and recordController.js for: GET /api/records/:teamId (filtered by role — Admin read-only, Team Head and Member their team), POST /api/records (Team Head + Member), PUT /api/records/:id (Team Head + Member — save edit history), DELETE /api/records/:id (Team Head only), GET /api/records/:id (single record with all column values, attachments, comments, history), PUT /api/records/:id/pin (Team Head only), POST /api/records/:id/attachments (Team Head + Member), GET /api/records/export/:teamId?format=csv (Admin + Team Head)"

---

### PHASE 49 — Build Task, Comment, Notification, and Activity APIs

- Ask Antigravity AI:
  - "Build backend/routes/tasks.js — GET /api/tasks (filtered by role), POST /api/tasks (Team Head), GET /api/tasks/:id, PUT /api/tasks/:id (Team Head + Member for status), DELETE /api/tasks/:id (Team Head)"
  - "Build backend/routes/comments.js — GET /api/comments?recordId= or ?taskId=, POST /api/comments, DELETE /api/comments/:id (own comment only)"
  - "Build backend/routes/notifications.js — GET /api/notifications (own), PUT /api/notifications/:id/read, PUT /api/notifications/read-all"
  - "Build backend/routes/activity.js — GET /api/activity/:teamId (Admin sees all teams, Team Head sees own team)"

---

### PHASE 50 — Connect Everything and Deploy

**Connect Frontend to Backend:**
- Ask Antigravity AI one section at a time:
  - "Set up src/api/axiosInstance.js — baseURL from VITE_API_URL, auto-attach JWT header, 401 → session expired redirect"
  - "Connect Login, ForgotPassword, ResetPassword, AcceptInvitation pages to auth API"
  - "Connect Admin pages: Dashboard (real data), UserManagement, InvitationTracker, TeamManagement, TeamRecords (dynamic columns), ColumnBuilder, TaskView, Reports, ActivityLog"
  - "Connect Team Head pages: Dashboard, TeamRecords (dynamic form), TaskBoard with drag-drop, TaskDetail, TeamMembers, ActivityLog"
  - "Connect Member pages: Dashboard, TeamRecords, MyTasks, TaskDetail"
  - "Connect NotificationsDropdown to notifications API — poll every 30 seconds or use Socket.io"
  - "Add Socket.io on frontend — listen for record:added, record:updated, task:updated events and refresh relevant data in real time"

**Deploy Backend to Render:**
- render.com → New Web Service → connect GitHub → Root: backend → Build: `npm install` → Start: `node server.js`
- Add all .env variables in Render dashboard → Deploy → Copy URL

**Deploy Frontend to Vercel:**
- vercel.com → New Project → connect GitHub → Root: frontend → Add VITE_API_URL = Render URL → Deploy → Copy URL

---

## CREDENTIALS REFERENCE

| Item | Value |
|------|-------|
| Admin Email | gowri.a@hicas.ac.in |
| Team 1 Head Email | gowritrendline@gmail.com |
| Team 2 Head Email | gowrihindusthan@gmail.com |
| Team 3 Head Email | gowricloud@gmail.com |

---

## PHASE CHECKLIST

| # | Phase | Status |
|---|-------|--------|
| 1 | Create All Accounts | ⬜ Not Started |
| 2 | Create Tracker Document | ⬜ Not Started |
| 3 | Create GitHub Repo and Set Up Project Folder | ⬜ Not Started |
| 4 | Create React Frontend Project | ⬜ Not Started |
| 5 | Set Up Frontend Folder Structure | ⬜ Not Started |
| 6 | Build Login Page UI | ⬜ Not Started |
| 7 | Build Forgot Password and Reset Password Pages UI | ⬜ Not Started |
| 8 | Build Accept Invitation Page UI | ⬜ Not Started |
| 9 | Build Sidebar and TopBar Layout Components | ⬜ Not Started |
| 10 | Build Dashboard Layout Wrapper | ⬜ Not Started |
| 11 | Build Admin Dashboard Page UI | ⬜ Not Started |
| 12 | Build Team Head Dashboard Page UI | ⬜ Not Started |
| 13 | Build Member Dashboard Page UI | ⬜ Not Started |
| 14 | Build User Management Page UI | ⬜ Not Started |
| 15 | Build Invite User Modal UI | ⬜ Not Started |
| 16 | Build Invitation Tracker Page UI | ⬜ Not Started |
| 17 | Build Team Management Page UI | ⬜ Not Started |
| 18 | Build Admin Team Data View Pages | ⬜ Not Started |
| 19 | Build Record Detail Page UI (Admin) | ⬜ Not Started |
| 20 | Build Column Builder Page UI | ⬜ Not Started |
| 21 | Build Add/Edit Column Modal UI | ⬜ Not Started |
| 22 | Build Task Board Page UI | ⬜ Not Started |
| 23 | Build Add/Edit Task Modal UI | ⬜ Not Started |
| 24 | Build Task Detail Page UI | ⬜ Not Started |
| 25 | Build Team Records Table Page UI (Team Head & Member) | ⬜ Not Started |
| 26 | Build Add/Edit Record Modal UI (Dynamic Form) | ⬜ Not Started |
| 27 | Build Record Detail Page UI (Team View) | ⬜ Not Started |
| 28 | Build Team Members Page UI | ⬜ Not Started |
| 29 | Build Activity Log Page UI | ⬜ Not Started |
| 30 | Build Reports Page UI (Admin) | ⬜ Not Started |
| 31 | Build My Tasks Page UI (Member) | ⬜ Not Started |
| 32 | Build Profile Settings Page UI | ⬜ Not Started |
| 33 | Build Notifications Dropdown UI | ⬜ Not Started |
| 34 | Build 404 Page and Session Expired Handling | ⬜ Not Started |
| 35 | Set Up React Router with Role-Based Protection | ⬜ Not Started |
| 36 | Add Auth Context and Protected Route | ⬜ Not Started |
| 37 | Create Supabase Project and Get Credentials | ⬜ Not Started |
| 38 | Create All Database Tables in Supabase | ⬜ Not Started |
| 39 | Insert Seed Data | ⬜ Not Started |
| 40 | Set Up Row Level Security | ⬜ Not Started |
| 41 | Create Backend Project | ⬜ Not Started |
| 42 | Set Up Backend Folder Structure and server.js | ⬜ Not Started |
| 43 | Create Backend .env File | ⬜ Not Started |
| 44 | Build Auth API | ⬜ Not Started |
| 45 | Build JWT Middleware and Role Guard | ⬜ Not Started |
| 46 | Build User and Invitation APIs | ⬜ Not Started |
| 47 | Build Team and Column Builder APIs | ⬜ Not Started |
| 48 | Build Records API (Dynamic) | ⬜ Not Started |
| 49 | Build Task, Comment, Notification, and Activity APIs | ⬜ Not Started |
| 50 | Connect Everything and Deploy | ⬜ Not Started |