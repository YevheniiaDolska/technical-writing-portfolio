# Tasks API Reference

## Overview

The Tasks API allows you to create, read, update, and delete tasks within projects. All task operations require authentication and appropriate permissions.

---

## Endpoints

### List Tasks

Returns a paginated list of tasks.

```http
GET /v1/tasks
```

**Query Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `project_id` | string | Filter by project |
| `status` | string | Filter by status (todo, in_progress, completed) |
| `assignee_id` | string | Filter by assigned user |
| `limit` | integer | Results per page (max: 100) |
| `offset` | integer | Pagination offset |

**Example Request:**
```
GET /v1/tasks?project_id=proj_123&status=in_progress
```

**Response:**
```json
{
  "data": [
    {
      "id": "task_abc123",
      "project_id": "proj_123",
      "name": "Update user documentation",
      "status": "in_progress",
      "priority": "high",
      "assignee_id": "user_456"
    }
  ],
  "pagination": {
    "total": 45,
    "limit": 20,
    "offset": 0
  }
}
```

---

### Get Task

Retrieve a specific task by ID.

```http
GET /v1/tasks/{task_id}
```

**Path Parameters:**

- `task_id` - The task identifier

**Response:**
```json
{
  "data": {
    "id": "task_abc123",
    "project_id": "proj_123",
    "name": "Update user documentation",
    "description": "Update the getting started guide with new screenshots",
    "status": "in_progress",
    "priority": "high",
    "assignee_id": "user_456",
    "created_at": "2024-01-20T10:30:00Z",
    "updated_at": "2024-01-22T14:15:00Z",
    "due_date": "2024-02-01T17:00:00Z"
  }
}
```

---

### Create Task

Create a new task in a project.

```http
POST /v1/tasks
```

**Request Body:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `project_id` | string | Yes | Project identifier |
| `name` | string | Yes | Task name (max 200 chars) |
| `description` | string | No | Detailed description |
| `priority` | string | No | Priority level (default: medium) |
| `assignee_id` | string | No | User to assign |
| `due_date` | datetime | No | Due date (ISO 8601) |

**Example Request:**
```json
{
  "project_id": "proj_123",
  "name": "Review API documentation",
  "description": "Technical review of the new endpoints",
  "priority": "high",
  "assignee_id": "user_789"
}
```

**Response:**
```json
{
  "data": {
    "id": "task_new456",
    "project_id": "proj_123",
    "name": "Review API documentation",
    "status": "todo",
    "created_at": "2024-01-25T11:00:00Z"
  }
}
```

---

### Update Task

Update an existing task. Use PATCH for partial updates.

```http
PATCH /v1/tasks/{task_id}
```

**Path Parameters:**

- `task_id` - The task identifier

**Request Body:**
Any task fields you want to update.

**Example Request:**
```json
{
  "status": "completed",
  "priority": "low"
}
```

**Response:**
Returns the updated task object.

---

### Delete Task

Permanently delete a task.

```http
DELETE /v1/tasks/{task_id}
```

**Path Parameters:**

- `task_id` - The task identifier

**Response:**
```
204 No Content
```

---

## Task Status Flow

Tasks can transition through the following statuses:

1. `todo` - Initial state
2. `in_progress` - Work has started
3. `review` - Pending review (optional)
4. `completed` - Task is done
5. `cancelled` - Task was cancelled

**Valid Transitions:**
- From `todo` → `in_progress`, `cancelled`
- From `in_progress` → `review`, `completed`, `todo`, `cancelled`
- From `review` → `completed`, `in_progress`
- From `completed` → No transitions allowed
- From `cancelled` → No transitions allowed

---

## Error Responses

### Task Not Found
```json
{
  "error": {
    "type": "not_found",
    "message": "Task not found",
    "code": "TASK_NOT_FOUND"
  }
}
```

### Validation Error
```json
{
  "error": {
    "type": "validation_error",
    "message": "Validation failed",
    "code": "ERR_VALIDATION",
    "details": {
      "fields": {
        "name": "Task name is required",
        "priority": "Invalid priority value"
      }
    }
  }
}
```

### Permission Denied
```json
{
  "error": {
    "type": "forbidden",
    "message": "You don't have permission to access this task",
    "code": "ERR_FORBIDDEN"
  }
}
```

---

## Best Practices

### Filtering and Pagination
- Always use pagination for list requests
- Combine filters to reduce response size
- Cache frequently accessed task lists

### Error Handling
- Check response status codes
- Parse error messages for user feedback
- Implement retry logic for 5xx errors

### Performance
- Request only needed fields when possible
- Use bulk operations for multiple updates
- Implement local caching where appropriate

---

## Related Documentation

- [Projects API](projects.md) - Managing projects
- [Users API](users.md) - User management
- [Webhooks](webhooks.md) - Real-time notifications

---

*Last updated: October 2025*