# Release Notes

## Version 2.0.0
*Released: October 15, 2025*

### Migration Checklist

Before upgrading to v2.0, complete these steps:

#### Pre-Migration (1 week before)
- [ ] Review breaking changes below
- [ ] Test your integration in sandbox environment
- [ ] Update date parsing to handle ISO 8601 format
- [ ] Check error handling for new response structure
- [ ] Identify deprecated endpoints in your code
- [ ] Plan maintenance window if needed

#### Migration Day
- [ ] Update API endpoint URLs if using deprecated ones
- [ ] Deploy updated error handling code
- [ ] Update API client libraries to v2.0
- [ ] Test critical workflows
- [ ] Monitor error rates for first hour

#### Post-Migration
- [ ] Verify all integrations working correctly
- [ ] Check webhook deliveries
- [ ] Review API response times
- [ ] Update documentation for your team
- [ ] Remove code for deprecated features

---

### New Features

**Batch Operations API**  
Process multiple operations in a single request. Create, update, or delete up to 100 resources at once with transactional processing.

**Advanced Task Dependencies**  
Define relationships between tasks with automatic status updates when dependencies complete. Includes circular dependency detection.

**Custom Fields**  
Add custom metadata to projects and tasks. Supports text, number, date, select, and multi-select field types with validation rules.

**Enhanced Webhooks**  
Improved webhook delivery with retry logic and signature verification. New events added: task.overdue, project.archived, team.updated.

### Improvements

**Performance**

- 30% faster response times for GET requests
- 50% improvement in task list query performance
- Optimized database indexing

**Rate Limits**

- Pro plan: Increased from 200/min to 300/min
- Enterprise plan: Increased from 600/min to 1000/min

**Pagination**

- Maximum page size increased to 100 items
- Added cursor-based pagination option
- Total count now included in all paginated responses

### API Changes

**Breaking Changes**

**1. Date Format Standardization**

- All timestamps now use ISO 8601 format
- Previous format: `2024-11-15 10:30:00`
- New format: `2024-11-15T10:30:00Z`

**2. Error Response Structure**

- Standardized error format across all endpoints
- Added `error.code` field for programmatic handling
- Validation errors now include field-level details

**3. Removed Endpoints**

- `GET /v1/projects/{id}/tasks` - Use `GET /v1/tasks?project_id={id}`
- `POST /v1/tasks/bulk` - Use `POST /v1/batch`

**Deprecations**

The following will be removed in v3.0:

- `X-RateLimit-Remaining` header (use `X-RateLimit-Limit`)
- `include` parameter (use `expand`)
- `filter` parameter (use specific query parameters)

### Bug Fixes

- Fixed incorrect assignee display after bulk updates
- Resolved pagination calculation errors
- Fixed webhook timeout handling
- Corrected timezone issues in recurring tasks
- Fixed memory leak in long-running connections

### Documentation Updates

- Added batch operations guide
- Updated authentication documentation
- New custom fields tutorial
- Improved error handling examples

---

## Version 1.9.0
*Released: October 1, 2025*

### New Features

- Task templates for common workflows
- Project archival functionality
- Improved search with fuzzy matching
- New webhook event: `project.milestone_reached`

### Improvements

- 20% faster project listing
- Reduced memory usage in exports
- Better validation error messages

### Bug Fixes

- Task assignment race condition
- Special characters in search
- Timezone handling in due dates

---

## Version 1.8.0
*Released: September 1, 2025*

### New Features

- Recurring tasks (daily, weekly, monthly)
- CSV import for bulk task creation
- Email notifications API
- Project templates

### Improvements

- Optimized queries for large projects
- Added request IDs to all responses
- Enhanced API documentation

### Bug Fixes

- Pagination with deleted items
- Webhook retry logic
- Project member count calculation

---

## Migration Guide

### Upgrading to v2.0

**Timeline**

- November 15, 2025: v2.0 released
- February 15, 2026: v1 marked deprecated
- May 15, 2026: v1 API sunset

**Required Changes**

1. Update date parsing to handle ISO 8601 format
2. Update error handling for new response structure
3. Replace deprecated endpoints with new equivalents
4. Update any hardcoded field names

**Testing Recommendations**

1. Test in sandbox environment first
2. Verify date/time handling
3. Check error response parsing
4. Test pagination changes

### Support

- **Migration assistance**: api-support@taskflow.example
- **Documentation**: docs.taskflow.example/migration
- **Status updates**: status.taskflow.example

---

*For complete version history, see [changelog.taskflow.example](https://changelog.taskflow.example)*