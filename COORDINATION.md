# Charge Captain - Multi-Instance Coordination Guide

This file coordinates work between multiple Claude instances working on different parts of the Charge Captain project.

## Project Structure

```
chargeCaptain/
├── chargeCaptainApi/          # Backend API (Express + MongoDB)
│   └── INSTANCE_INSTRUCTIONS_API.md
├── chargeCaptainDashboard/    # Web admin portal (React)
│   └── INSTANCE_INSTRUCTIONS_DASHBOARD.md
├── cc_mob/                    # Mobile app (Flutter)
│   └── CLAUDE.md
├── COORDINATION.md            # This file
└── WORK_STATUS.md             # Current work status (update frequently)
```

## Active Instances

### Instance A - Dashboard Development
- **Scope**: Frontend dashboard (React/Vite)
- **Working Directory**: `chargeCaptainDashboard/`
- **Instructions**: See `chargeCaptainDashboard/INSTANCE_INSTRUCTIONS_DASHBOARD.md`
- **Files to modify**: `src/**`
- **Files to NOT modify**: `../chargeCaptainApi/**`, `../cc_mob/**`

### Instance B - API Development
- **Scope**: Backend API (Express/MongoDB)
- **Working Directory**: `chargeCaptainApi/`
- **Instructions**: See `chargeCaptainApi/INSTANCE_INSTRUCTIONS_API.md`
- **Files to modify**: `server/**`, `docs/**`
- **Files to NOT modify**: `../chargeCaptainDashboard/**`, `../cc_mob/**`

### Instance C - Mobile Development
- **Scope**: Mobile app (Flutter/Dart)
- **Working Directory**: `cc_mob/`
- **Instructions**: See `cc_mob/CLAUDE.md`
- **Files to modify**: `lib/**`
- **Files to NOT modify**: `../chargeCaptainApi/**`, `../chargeCaptainDashboard/**`

## Coordination Rules

### 1. File Ownership
- **Dashboard Instance**: Only modifies files in `chargeCaptainDashboard/`
- **API Instance**: Only modifies files in `chargeCaptainApi/`
- **Mobile Instance**: Only modifies files in `cc_mob/`
- **All**: Can read each other's documentation files

### 2. Communication Protocol
- Update `WORK_STATUS.md` when:
  - Starting a new task
  - Completing a task
  - Discovering an issue that affects the other instance
  - Creating a new API endpoint or changing an existing one
  - Modifying data structures or response formats

### 3. Dependency Management
- **API changes first**: If a feature requires both API and dashboard changes, implement API endpoint first
- **Test independently**: Test API endpoints with Postman/Swagger before dashboard integration
- **Document changes**: Update API documentation immediately after changes

### 4. Handoff Process

**When API instance creates a new endpoint:**
1. Implement and test the endpoint
2. Update Swagger/JSDoc documentation
3. Add entry to `docs/api-quick-reference.md`
4. Update `WORK_STATUS.md` with:
   - Endpoint URL
   - Method (GET/POST/PUT/DELETE)
   - Request body schema
   - Response schema
   - Example usage

**When Dashboard instance needs an API endpoint:**
1. Check `WORK_STATUS.md` to see if it's already implemented
2. If not, add request to `WORK_STATUS.md` under "API Endpoint Requests - Admin (`/api/a/*`)"
3. Wait for API instance to implement
4. Check `WORK_STATUS.md` for completion notice
5. Read updated API documentation
6. Implement frontend integration

**When Mobile instance needs an API endpoint:**
1. Check `WORK_STATUS.md` to see if it's already implemented
2. If not, add request to `WORK_STATUS.md` under "API Endpoint Requests - Client User (`/api/cu/*`)"
3. Specify mobile-specific requirements (e.g., optimized response size, image upload format)
4. Wait for API instance to implement
5. Check `WORK_STATUS.md` for completion notice
6. Read updated API documentation
7. Implement mobile integration with Flutter/Dio

## Conflict Resolution

### If both instances need to modify shared files:
1. **Documentation files** (`CLAUDE.md`, `README.md`):
   - Instance that completes work first updates the file
   - Second instance reads the update before making changes
   - Use git to merge if necessary

2. **Schema/Model changes**:
   - API instance owns the schema
   - Dashboard instance requests changes via `WORK_STATUS.md`

3. **Environment variables**:
   - API instance defines required variables
   - Dashboard instance documents usage

## Current Work Status

See `WORK_STATUS.md` for real-time status updates.

## Integration Points

### Authentication
- **API provides**:
  - Admin JWT tokens via `/api/a/login` (for Dashboard)
  - Client User JWT tokens via `/api/cu/validateOTP` (for Mobile)
- **Dashboard consumes**: Admin JWT, stores in localStorage, sends in Authorization header
- **Mobile consumes**: Client User JWT, stores in FlutterSecureStorage, sends in Authorization header
- **Coordination**: If auth flow changes, API instance MUST update WORK_STATUS.md

### Data Formats

#### Site Object
**API provides:**
```javascript
{
  _id: "string",
  name: "string",
  location: { type: "Point", coordinates: [lng, lat] },
  address: "string",
  cpoName: "string",
  stations: [...],
  operationalStatus: "string",
  googlePlaceId: "string",
  plugshareLocationId: "number"
}
```

**Dashboard expects:** Same format (uses transformSiteData in src/api/sites.js)

#### User Object
**API provides:**
```javascript
{
  _id: "string",
  name: "string",
  email: "string",
  phoneNo: "string",
  lifetimePoints: number,
  totalAvailablePoints: number,
  vehicles: [...]
}
```

**Dashboard expects:** Same format

### Pagination Format
**API provides:**
```javascript
{
  data: [...],
  page: number,
  pages: number,
  count: number,
  total: number,
  stats: { ... }
}
```

**Dashboard expects:** Same format (handled in src/api/*.js)

## Testing Checklist

Before marking work as complete:

### API Instance
- [ ] Endpoint tested with Postman/Swagger
- [ ] Response format documented
- [ ] Error cases handled
- [ ] Swagger documentation updated
- [ ] `docs/api-quick-reference.md` updated
- [ ] `WORK_STATUS.md` updated with completion notice
- [ ] Ran `npm test` (if tests exist)

### Dashboard Instance
- [ ] Component renders without errors
- [ ] API integration works with actual backend
- [ ] Loading states implemented
- [ ] Error handling implemented
- [ ] Responsive design checked
- [ ] `WORK_STATUS.md` updated with completion notice
- [ ] Ran `pnpm build` successfully

### Mobile Instance
- [ ] Screen renders without errors on Android/iOS
- [ ] API integration works with actual backend (`/api/cu/*` endpoints)
- [ ] Loading states implemented
- [ ] Error handling implemented
- [ ] Image upload and location permissions working
- [ ] Secure storage handling working
- [ ] `WORK_STATUS.md` updated with completion notice
- [ ] Ran `flutter analyze` with no issues
- [ ] Tested on physical device (not just emulator)

## Emergency Procedures

### If API instance breaks the dashboard or mobile:
1. API instance: Immediately update `WORK_STATUS.md` with "BREAKING CHANGE" notice
2. API instance: Document what changed and migration path
3. API instance: Specify which endpoints are affected (`/api/a/*` or `/api/cu/*`)
4. Dashboard/Mobile instance: Read breaking change notice
5. Dashboard/Mobile instance: Implement required changes
6. All: Test integration

### If dashboard or mobile instance discovers API bug:
1. Dashboard/Mobile instance: Document bug in `WORK_STATUS.md` under "API Bugs"
2. Dashboard/Mobile instance: Include steps to reproduce
3. Dashboard/Mobile instance: Specify which endpoint is affected
4. API instance: Fix bug and update `WORK_STATUS.md`
5. Dashboard/Mobile instance: Verify fix

## File-Based Flags

Use these files for simple coordination:

```bash
# API instance signals endpoint is ready
touch READY_SITES_EXPORT_ENDPOINT

# Dashboard instance signals feature is complete
touch READY_SITES_MAP_VIEW

# Either instance signals blocking issue
echo "Cannot proceed: Missing authentication middleware" > BLOCKED_REWARDS_FEATURE
```

## Best Practices

1. **Read before write**: Always check `WORK_STATUS.md` before starting work
2. **Update frequently**: Update `WORK_STATUS.md` at least every 30 minutes during active work
3. **Be specific**: Include file paths, line numbers, and examples in updates
4. **Test integration**: Don't assume compatibility - test with actual API/frontend
5. **Document assumptions**: If you assume something about the other instance's work, document it

## Example Workflow

### Feature: Export Sites to CSV

**Step 1 - Dashboard Instance:**
```markdown
# Update WORK_STATUS.md
## Dashboard Instance - Work In Progress
- Feature: Export sites to CSV
- Status: Needs API endpoint
- Added to: src/features/sites/SitesListPage.jsx (button placeholder)

## API Endpoint Requests
- GET /api/a/export/sites?format=csv
  - Should return CSV file with all sites
  - Columns: name, address, cpoName, operationalStatus, googlePlaceId, stations count
  - Content-Type: text/csv
```

**Step 2 - API Instance:**
```markdown
# Update WORK_STATUS.md
## API Instance - Work In Progress
- Feature: Sites CSV export endpoint
- Status: Implementing
- Files: server/controllers/exportController.js, server/routes/adminRoutes.js

## API Instance - Completed
- ✅ GET /api/a/export/sites?format=csv
- Response: CSV file download
- Headers: Content-Type: text/csv, Content-Disposition: attachment; filename="sites.csv"
- Example: curl http://localhost:3001/api/a/export/sites?format=csv
- Documented in: docs/api-quick-reference.md line 234
```

**Step 3 - Dashboard Instance:**
```markdown
# Update WORK_STATUS.md
## Dashboard Instance - Completed
- ✅ Export sites to CSV
- Integrated with GET /api/a/export/sites
- Files modified: src/features/sites/SitesListPage.jsx, src/api/sites.js
- Tested: Downloads CSV file successfully with 1000+ sites
```

## Version Control Tips

### For parallel work on different features:
```bash
# API Instance
git checkout -b feature/csv-export-api
# Work on API
git commit -m "Add CSV export endpoint"
git push origin feature/csv-export-api

# Dashboard Instance
git checkout -b feature/csv-export-dashboard
# Work on dashboard
git commit -m "Add CSV export button"
git push origin feature/csv-export-dashboard

# When both complete, merge in sequence:
# 1. Merge API branch first
# 2. Then merge Dashboard branch
```

## Common Pitfalls

1. **Don't assume endpoint exists**: Always check API documentation before implementing frontend
2. **Don't change response format without notice**: Breaking changes must be documented
3. **Don't skip testing**: Test with actual API/frontend, not just assumptions
4. **Don't forget to update status**: Other instance is blocked waiting for your update

## Quick Reference

- **Status file**: `WORK_STATUS.md`
- **API docs**: `chargeCaptainApi/CLAUDE.md`
- **API reference**: `chargeCaptainApi/docs/api-quick-reference.md`
- **Dashboard docs**: `chargeCaptainDashboard/CLAUDE.md`
- **Dashboard reference**: `DASHBOARD_REFERENCE.md`
- **Mobile docs**: `cc_mob/CLAUDE.md`

---

**Last Updated**: 2024-12-19
**Coordination Protocol Version**: 1.0
