# TaskFlow API Documentation

## Overview

The TaskFlow API provides programmatic access to project and task management functionality. This RESTful API allows you to integrate TaskFlow with your applications and automate workflows.

**Base URL:** `https://api.taskflow.example/v1`  
**Protocol:** HTTPS only  
**Data Format:** JSON

---

## Use Cases

### Common Integration Scenarios

**Project Management Automation**

- Automatically create projects from your CRM when deals are won
- Sync tasks between TaskFlow and your calendar
- Generate weekly reports from project data

**Development Workflow Integration**

- Create tasks from GitHub issues
- Update task status when code is deployed
- Link commits to specific tasks

**Business Intelligence**

- Export task data for analytics
- Track team productivity metrics
- Monitor project timelines and bottlenecks

**Third-party Tool Integration**

- Connect with Slack for notifications
- Integrate with time tracking tools
- Sync with accounting software for project billing

---

## Getting Started

### Prerequisites

Before using the API, ensure you have:

**1. TaskFlow Account**

- Sign up at [taskflow.example](https://taskflow.example)
- Verify your email address
- Choose appropriate plan for API access

**2. API Key**

- Log in to TaskFlow dashboard
- Navigate to Settings → API Keys
- Generate and securely store your key

**3. Development Environment**

- HTTP client (Postman, Insomnia, or curl)
- Programming language with HTTP library (optional)
- JSON parser for handling responses

---

## Authentication

All API requests require authentication using an API key in the Authorization header:

```
Authorization: Bearer YOUR_API_KEY
```

**Note:** The word "Bearer" must be included before your API key.

### Example Request

```
GET https://api.taskflow.example/v1/projects
Authorization: Bearer tfk_live_abc123def456ghi789
```

### Security Best Practices

- Store API keys in environment variables, not in code
- Use different keys for development and production
- Rotate keys regularly (every 90 days recommended)
- Never share keys in public repositories

---

## Request Structure

### HTTP Methods

| Method | Purpose | Example Use |
|--------|---------|-------------|
| GET | Retrieve data | Get list of tasks |
| POST | Create new resource | Create a new project |
| PUT | Replace entire resource | Update all task fields |
| PATCH | Update specific fields | Change task status only |
| DELETE | Remove resource | Delete a project |

### Required Headers

| Header | Value | When Required |
|--------|-------|---------------|
| Authorization | Bearer YOUR_API_KEY | Always |
| Content-Type | application/json | POST, PUT, PATCH |
| Accept | application/json | Recommended |

---

## Response Structure

### Successful Response

```json
{
  "data": {
    "id": "task_123",
    "name": "Complete documentation",
    "status": "in_progress"
  },
  "meta": {
    "request_id": "req_abc123",
    "timestamp": "2024-01-25T10:00:00Z"
  }
}
```

**Fields Explained:**

- `data` - The actual response content
- `meta` - Additional information about the request
- `request_id` - Unique identifier for troubleshooting

### Error Response

```json
{
  "error": {
    "type": "validation_error",
    "message": "The name field is required",
    "code": "FIELD_REQUIRED"
  }
}
```

**Error Fields:**

- `type` - Category of error (validation, authentication, etc.)
- `message` - Human-readable description
- `code` - Machine-readable code for programmatic handling

---

## Core Resources

### Projects

Projects are containers for organizing related tasks.

**Key Attributes:**

- `id` - Unique identifier (e.g., "proj_abc123")
- `name` - Project title (required, 1-100 characters)
- `description` - Detailed description (optional)
- `status` - Current state: active, on_hold, completed, archived
- `created_at` - When project was created (ISO 8601 format)

### Tasks

Tasks represent individual work items within projects.

**Key Attributes:**

- `id` - Unique identifier (e.g., "task_xyz789")
- `project_id` - Parent project (required)
- `name` - Task title (required, 1-200 characters)
- `status` - Current state: todo, in_progress, review, completed
- `priority` - Urgency level: low, medium, high, urgent
- `assignee_id` - ID of assigned user (optional)
- `due_date` - Deadline in ISO 8601 format (e.g., "2024-01-25T17:00:00Z")

### Users

Users who can be assigned to tasks and manage projects.

**Key Attributes:**

- `id` - Unique identifier (e.g., "user_123")
- `email` - User's email address
- `name` - Display name
- `role` - Permission level: admin, member, viewer

---

## HTTP Status Codes

Standard HTTP status codes indicate request results:

### Success Codes (2xx)

| Code | Meaning | When Used |
|------|---------|-----------|
| 200 | OK | Successful GET, PUT, PATCH |
| 201 | Created | Successful POST (new resource created) |
| 204 | No Content | Successful DELETE |

### Client Error Codes (4xx)

| Code | Meaning | Common Causes |
|------|---------|---------------|
| 400 | Bad Request | Malformed JSON, invalid parameters |
| 401 | Unauthorized | Missing or invalid API key |
| 403 | Forbidden | Valid key but insufficient permissions |
| 404 | Not Found | Resource doesn't exist or you can't access it |
| 422 | Unprocessable Entity | Validation failed (e.g., missing required field) |
| 429 | Too Many Requests | Rate limit exceeded |

### Server Error Codes (5xx)

| Code | Meaning | What To Do |
|------|---------|------------|
| 500 | Internal Server Error | Retry after a few seconds |
| 503 | Service Unavailable | Service is down, check status page |

---

## Rate Limiting

API requests are limited to prevent abuse and ensure service quality.

### Rate Limits by Plan

| Plan | Requests per Hour | Burst Limit |
|------|------------------|-------------|
| Free | 1,000 | 50/minute |
| Professional | 10,000 | 200/minute |
| Enterprise | 100,000 | 1000/minute |

### Rate Limit Headers

Every response includes rate limit information:

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 950
X-RateLimit-Reset: 1706180400
```

**Header Definitions:**

- `Limit` - Your maximum requests per hour
- `Remaining` - How many requests you have left
- `Reset` - When your limit resets (Unix timestamp = seconds since Jan 1, 1970)

### Handling Rate Limits

When you exceed the limit:
1. API returns `429 Too Many Requests`
2. Check `X-RateLimit-Reset` header
3. Wait until reset time
4. Retry your request

---

## Pagination

Large data sets are split into pages for better performance.

### Pagination Parameters

| Parameter | Purpose | Example |
|-----------|---------|---------|
| `limit` | Items per page (max: 100) | `limit=20` |
| `offset` | Items to skip | `offset=40` (skip first 40) |
| `sort` | Field to sort by | `sort=created_at` |
| `order` | Sort direction | `order=desc` (newest first) |

### Example: Getting Page 3

To get the third page of 20 items:
```
GET /v1/tasks?limit=20&offset=40
```

This skips the first 40 items (pages 1-2) and returns items 41-60.

### Pagination Response

```json
{
  "data": [...],
  "pagination": {
    "total": 150,
    "limit": 20,
    "offset": 40,
    "has_more": true
  }
}
```

---

## Error Handling

### Error Types

| Type | Description | How to Fix |
|------|-------------|------------|
| `authentication_error` | Invalid API key | Check your API key |
| `authorization_error` | No permission | Check user role/permissions |
| `validation_error` | Invalid data | Review required fields |
| `not_found` | Resource missing | Verify ID exists |
| `rate_limit` | Too many requests | Wait and retry |
| `server_error` | Server problem | Retry with exponential backoff |

### Validation Error Details

Validation errors include field-specific information:

```json
{
  "error": {
    "type": "validation_error",
    "message": "Validation failed",
    "details": {
      "name": "Name is required",
      "due_date": "Date must be in the future"
    }
  }
}
```

---

## Best Practices

### Performance Tips

**Use Pagination**
- Request only the data you need
- Start with smaller page sizes (20-50 items)
- Increase if needed, but stay under 100

**Filter Your Requests**
- Use query parameters to get specific data
- Combine filters to reduce response size
- Example: `GET /v1/tasks?status=in_progress&priority=high`

### Error Recovery

**Implement Retry Logic**
- For 5xx errors (server errors), wait and retry
- Use exponential backoff: wait 1s, then 2s, then 4s, etc.
- Stop after 3-5 attempts

**Handle Rate Limits Gracefully**
- Check rate limit headers before making many requests
- Implement request queuing if needed
- Consider upgrading plan for higher limits

---

## Support Resources

### Documentation
- **API Reference:** [Full endpoint details](https://yevheniiadolska.github.io/technical-writing-portfolio/api-documentation/api-endpoints/)
- **Status Page:** [status.taskflow.example](https://status.taskflow.example)
- **Changelog:** [changelog.taskflow.example](https://changelog.taskflow.example)

### Getting Help
- **Email:** api-support@taskflow.example
- **Response Time:** Within 24 hours (business days)
- **Include:** Request ID, endpoint, error message

---

*API Version: 1.0*  
*Last Updated: October 2025*