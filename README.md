# Kanban Task Manager

A full-stack Kanban project and task manager built by a 4-person team for CSC 413 (Software Development) at San Francisco State University.

The team repo is private, so this repo summarizes the project and the part I built: **task management and the Kanban board**.

**Tech stack:** Java 17 · Spring Boot · MongoDB · Next.js (App Router)

---

## The project

Users sign in, create projects, and manage each project's tasks on a Kanban board.

| Feature | What it does |
|---|---|
| Auth | Register and log in; login sets an auth cookie |
| Projects | Create projects and open each one's board |
| **Kanban board** | **Add tasks, move them between To Do / In Progress / Done, assign a user, delete** |
| Search | Filter a project's tasks by keyword, status, or priority, with saved searches |
| Profile | Edit display name and bio |
| Activity feed | Logs project and task events, newest first |

**Architecture:** the Next.js front end proxies `/api/*` requests to a Spring Boot REST API on port 8080, which stores data in MongoDB.

---

## My contribution: task management

I owned the task feature end to end, from the database collection to the board UI.

### Back end: REST endpoints (Spring Boot)

| Method | Path | What it does |
|---|---|---|
| POST | `/tasks/create` | Create a task (starts in `todo`) |
| GET | `/tasks/project/{projectId}` | List all tasks in a project |
| PUT | `/tasks/{id}` | Update status, title, priority, or assignee |
| DELETE | `/tasks/{id}` | Delete a task |

- `PUT` only updates the fields that are sent, rejects invalid statuses (400), and returns 404 for missing tasks
- `DELETE` returns 404 if the task doesn't exist
- Example create request:

```json
{
  "projectId": "...",
  "title": "Design login page",
  "description": "...",
  "priority": "medium",
  "createdByUserName": "alice"
}
```

### Database: `tasks` collection (MongoDB)

Designed the `Task` document that the search and activity features also build on:

| Field | Notes |
|---|---|
| `id` | MongoDB document ID |
| `projectId` | **Indexed**, so loading a project's tasks stays fast |
| `title`, `description`, `priority` | Set when the task is created |
| `status` | Starts as `todo`; one of `todo`, `in_progress`, `done` |
| `createdByUserName`, `assigneeUserName` | Who made the task and who it's assigned to |
| `createdAt`, `updatedAt` | Timestamps, with `updatedAt` refreshed on every edit |

### Front end: `/board` (Next.js)

- 3-column board: **To Do**, **In Progress**, **Done**, each with a task count
- Loads a project's tasks from `?projectId=`
- Add tasks with a title, description, and priority
- Tasks move one step at a time: To Do ↔ In Progress ↔ Done
- Assign a user and delete tasks; the board reloads after every change

---

## Walkthrough

[demo](https://github.com/user-attachments/assets/da5ac767-06e8-456b-8fe0-b0d4040bc65e)
## Screenshots

soon
---

*Team project, CSC 413, San Francisco State University. Built with three teammates who owned auth and projects, search, and profile and activity.*
