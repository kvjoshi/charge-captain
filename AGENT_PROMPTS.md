# Agent Communication Prompts

This file contains ready-to-use prompts for coordinating work between different Claude instances (Mobile, Backend API, Dashboard).

**Last Updated**: 2026-02-18

---

## How to Use This File

When an agent identifies work needed by another agent, copy the relevant prompt below and paste it to the appropriate agent.

---

## Templates

### New API Endpoint Request (Mobile/Dashboard → Backend API)

```
I need you to implement a new API endpoint.

**Check WORK_STATUS.md first:**
- Location: d:\Clients\chargeCaptain\WORK_STATUS.md
- Look for the endpoint specification documented there

**After completion:**
1. Update WORK_STATUS.md with completion status
2. Document the endpoint in `docs/api-quick-reference.md`
3. Add Swagger JSDoc to the controller

**Files you'll typically modify:**
- `chargeCaptainApi/server/controllers/[controller].js`
- `chargeCaptainApi/server/routes/adminRoutes.js` (or clientUserRoutes.js)
- `chargeCaptainApi/docs/api-quick-reference.md`
```

### Dashboard Feature Request (Any → Dashboard)

```
I need you to implement a dashboard feature.

**Check WORK_STATUS.md first:**
- Location: d:\Clients\chargeCaptain\WORK_STATUS.md
- Look for the feature specification documented there

**After completion:**
Update WORK_STATUS.md with completion status.
```

### API Bug Report (Any → Backend API)

```
I've discovered a bug in the API.

**Check WORK_STATUS.md:**
- Section: "Known Issues"
- Full bug report documented there

**Quick Summary:**
- Endpoint: [affected endpoint]
- Issue: [brief description]
- Impact: [what features are blocked]
```

---

## Completed Prompts Archive

### Profile Update Endpoint (2026-01-26) — COMPLETED
- **Endpoint**: `POST /api/cu/updateProfile`
- **Status**: Implemented and integrated
- **See**: WORK_STATUS.md → API Task 2

### Excel Site Import (2026-02-18) — COMPLETED
- **API Endpoint**: `POST /api/a/upload-excel-sites` — DONE
- **Dashboard UI**: Excel Import tab in PlugShare Import page — DONE
- **See**: WORK_STATUS.md → "Excel Site Import Feature"

---

## Communication Protocol

**File Ownership Rules:**
- **API Agent**: Only modifies files in `chargeCaptainApi/`
- **Dashboard Agent**: Only modifies files in `chargeCaptainDashboard/`
- **Mobile Agent**: Only modifies files in `cc_mob/`
- **All**: Update `WORK_STATUS.md` for coordination

**Workflow:**
```
Agent A → Documents need in WORK_STATUS.md
     ↓
User copies prompt from AGENT_PROMPTS.md → pastes to Agent B
     ↓
Agent B → Reads WORK_STATUS.md → Implements → Updates WORK_STATUS.md
```

**File Locations:**
- WORK_STATUS.md: `d:\Clients\chargeCaptain\WORK_STATUS.md`
- API CLAUDE.md: `d:\Clients\chargeCaptain\chargeCaptainApi\CLAUDE.md`
- Dashboard CLAUDE.md: `d:\Clients\chargeCaptain\chargeCaptainDashboard\CLAUDE.md`
- Mobile CLAUDE.md: `d:\Clients\chargeCaptain\cc_mob\CLAUDE.md`

---

**End of Agent Communication Prompts**
