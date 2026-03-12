# Agent Communication Prompts

This file contains ready-to-use prompts for coordinating work between different Claude instances (Mobile, Backend API, Dashboard).

**Last Updated**: 2026-01-26

---

## How to Use This File

When the Mobile Agent identifies work needed by other agents, copy the relevant prompt below and paste it to the appropriate agent.

---

## 📱 Mobile → Backend API Agent

### Template: New API Endpoint Request

```
I need you to implement a new API endpoint for the mobile app.

**Check WORK_STATUS.md first:**
- Location: d:\Clients\chargeCaptain\WORK_STATUS.md
- Section: "API Endpoint Requests"
- Look for the endpoint specification I've documented

**Endpoint Details:**
[The Mobile Agent will have documented the full specification in WORK_STATUS.md]

**After completion:**
1. Update WORK_STATUS.md → "API Endpoint Completions" section
2. Document the endpoint in `docs/api-quick-reference.md`
3. Update your status in "Current Active Work"

**Files you'll need to modify:**
- `chargeCaptainApi/server/controllers/[controller].js`
- `chargeCaptainApi/server/routes/clientUserRoutes.js` (or adminRoutes.js)
- `chargeCaptainApi/docs/api-quick-reference.md`
```

### Current Request: Profile Update Endpoint (2026-01-26)

```
Implement POST /api/cu/updateProfile endpoint.

**Full specs:** WORK_STATUS.md lines 389-406

**Quick Summary:**
- Endpoint: POST /api/cu/updateProfile
- Auth: clientUserProtect middleware (JWT required)
- Purpose: Allow users to edit profile after onboarding
- Request Body:
  {
    "name": "John Doe",      // Optional
    "phone": "+919876543210", // Optional
    "city": "Mumbai"         // Optional
  }
- Response: Updated user object
- Validation:
  - name: min 2 chars, max 100 chars, letters/spaces only
  - phone: valid Indian format (+91XXXXXXXXXX or 10 digits)
  - city: min 2 chars, max 50 chars
- Note: Current /onboard endpoint only works when user.onboarding === true
  This new endpoint should work at any time post-onboarding

**Files to modify:**
- server/controllers/clientUserController.js (add updateProfile function)
- server/routes/clientUserRoutes.js (register route)
- docs/api-quick-reference.md (document endpoint)

**After completion:**
Update WORK_STATUS.md → "API Endpoint Completions" section with:
- Endpoint ready status
- Example request/response
- Any important notes for mobile integration
```

---

## 📱 Mobile → Dashboard Agent

### Template: Dashboard Feature Request

```
I need you to implement a dashboard feature that's needed for the mobile app workflow.

**Check WORK_STATUS.md first:**
- Location: d:\Clients\chargeCaptain\WORK_STATUS.md
- Section: "Dashboard Requests" (if applicable)
- Look for the feature specification I've documented

**Feature Details:**
[Description of what needs to be added to the dashboard]

**Why this is needed:**
[Explanation of mobile app dependency]

**After completion:**
Update WORK_STATUS.md → "Dashboard Completions" section
```

---

## 🔧 Backend API → Mobile Agent

### Template: API Endpoint Ready Notification

```
The API endpoint you requested is now ready for integration.

**Endpoint:** [endpoint path]
**Status:** ✅ READY FOR INTEGRATION
**Documentation:** See WORK_STATUS.md → "API Endpoint Completions"

**Mobile Integration Steps:**
[Steps the mobile agent should follow]

**Testing:**
[Any testing notes or sample requests]
```

---

## 🐛 Bug Report: Mobile → Backend API

### Template: API Bug Report

```
I've discovered a bug in the API while working on the mobile app.

**Check WORK_STATUS.md:**
- Section: "Known Issues" → "API Bugs (Discovered by Mobile)"
- I've documented the full bug report there

**Quick Summary:**
- Endpoint: [affected endpoint]
- Issue: [brief description]
- Expected behavior: [what should happen]
- Actual behavior: [what's happening]
- Impact: [what mobile features are blocked]

**Full details in WORK_STATUS.md** including:
- Steps to reproduce
- Sample request/response
- Error messages
- Mobile files blocked
```

---

## 📋 Status Update Templates

### Mobile Agent Status Update

Use these when updating your status in WORK_STATUS.md:

**Starting new work:**
```markdown
### Mobile Instance - Status
- **Working On**: [Feature name]
- **Status**: 🔄 IN PROGRESS
- **Last Update**: [YYYY-MM-DD]
- **Current Progress**:
  - 🔄 Task X: [Description] (IN PROGRESS)
  - ⏳ Task Y: [Description] (BLOCKED - waiting for [dependency])
```

**Completed work:**
```markdown
### Mobile Instance - Status
- **Working On**: [Feature name] - Task X Complete
- **Status**: ✅ COMPLETED / ⏳ WAITING
- **Last Update**: [YYYY-MM-DD]
- **Completed**:
  - ✅ Task X: [Description] (DONE)
- **Next**:
  - ⏳ Task Y: [Description] (Blocked by [dependency])
```

---

## 🎯 Quick Reference: When to Use Each Agent

| Task Type | Agent | Prompt Section |
|-----------|-------|----------------|
| New API endpoint | Backend API | "Mobile → Backend API Agent" |
| API bug fix | Backend API | "Bug Report: Mobile → Backend API" |
| Dashboard feature | Dashboard | "Mobile → Dashboard Agent" |
| Mobile UI/feature | Mobile | (You handle this directly) |
| API documentation | Backend API | (Part of endpoint implementation) |
| Testing coordination | All agents | Custom - coordinate via WORK_STATUS.md |

---

## 📝 Communication Protocol Reminder

**For Mobile Agent (You):**
1. ❌ NEVER edit API code (`chargeCaptainApi/`)
2. ❌ NEVER edit Dashboard code (`chargeCaptainDashboard/`)
3. ✅ ALWAYS use WORK_STATUS.md to communicate needs
4. ✅ ALWAYS tell user to forward work to other agents
5. ✅ ALWAYS provide clear, specific requirements

**File Locations:**
- WORK_STATUS.md: `d:\Clients\chargeCaptain\WORK_STATUS.md`
- Mobile CLAUDE.md: `d:\Clients\chargeCaptain\cc_mob\CLAUDE.md`
- API CLAUDE.md: `d:\Clients\chargeCaptain\chargeCaptainApi\CLAUDE.md`
- Dashboard CLAUDE.md: `d:\Clients\chargeCaptain\chargeCaptainDashboard\CLAUDE.md`

---

## 💡 Pro Tips for User

**When Mobile Agent says:**
> "Please ask the Backend Agent to implement X"

**You should:**
1. Open this file (AGENT_PROMPTS.md)
2. Find the relevant prompt template
3. Copy the prompt
4. Paste to Backend Agent instance
5. The agent will coordinate via WORK_STATUS.md

**Workflow:**
```
Mobile Agent → Documents need in WORK_STATUS.md
     ↓
User copies prompt from AGENT_PROMPTS.md
     ↓
Backend Agent → Reads WORK_STATUS.md → Implements → Updates WORK_STATUS.md
     ↓
Mobile Agent → Reads WORK_STATUS.md → Sees completion → Integrates
```

---

## 📞 Example Conversation Flow

**Scenario:** Mobile needs API endpoint

```
Mobile Agent: "I need the Backend Agent to create POST /api/cu/updateProfile.
               I've documented the specs in WORK_STATUS.md lines 389-406."

User: [Copies prompt from AGENT_PROMPTS.md]
      [Pastes to Backend Agent]

Backend Agent: "I'll implement the endpoint as specified in WORK_STATUS.md."
               [Works on implementation]
               [Updates WORK_STATUS.md when done]

Mobile Agent: [Reads WORK_STATUS.md]
              "I see the API endpoint is ready! I'll now integrate it."
              [Implements mobile integration]
```

---

## 🔄 Excel Site Import Feature (2026-02-17)

### Backend API Agent Prompt — Excel Import Endpoint

```
## Task: Implement Excel Site Import Endpoint

### Context
We have a data collector who provides charging station data as an Excel (.xlsx) file. Each row represents one station. The "Place Details" column contains Google Drive share links pointing to PlugShare-format JSON files with full station details. We need an endpoint that parses the Excel, downloads each JSON from Drive, and imports the stations.

### What to Build

New endpoint: POST /api/a/upload-excel-sites

**Upload**: Single file via multer — field name `excelFile` (.xlsx file)

**Flow:**
1. Parse the Excel file using the `xlsx` library (SheetJS)
2. For each row, extract the Google Drive file ID from the "Place Details" column
3. Download the JSON from Google Drive: `https://drive.google.com/uc?export=download&id={FILE_ID}`
4. Pass the downloaded JSON directly to the existing `decode()` function from `server/helpers/decodePlugshare.js` — the JSONs are raw PlugShare format (flat structure with fields like `name`, `latitude`, `longitude` at root level) which decode() already handles perfectly
5. Enrich the decoded site with Excel metadata (see enrichment rules below)
6. Upsert by `metadata.plugShareId` — same pattern as `uploadPlugShareData()` in siteController.js
7. Return import summary

**Excel Column Layout (first sheet, header row 1):**
| Column Header | Usage |
|---|---|
| Sr No: | Row number (for logging) |
| Place Name(Station Name) | Station name (for logging/warnings) |
| Place Details | Google Drive link to PlugShare JSON file |
| City Name | City name |
| Place Id | Google Places ID, OR coordinates (like "28.557, 77.164"), OR notes like "Miss spelled" |
| Company Name | CPO (Charge Point Operator) |
| Plug type: | Connector types, comma-separated (e.g., "CCS2, Type 2") |
| Station Status | "Coming soon", "Under repair", or empty |
| Plugshare Location | PlugShare ID number (may be empty for some rows) |

**Google Drive Link Parsing:**
Links look like: `https://drive.google.com/file/d/1B82ipR6R8sr2RKYCjI890ZLe5WktNnzs/view?usp=drive_link`
Extract file ID between `/d/` and `/view` → download from `https://drive.google.com/uc?export=download&id={FILE_ID}`
One row (row 72) has a local filename instead of Drive link — skip those with a warning.

**Enrichment Rules (apply on top of decoded JSON data):**
- `CPO` ← Company Name column (always override)
- `location.placeId` ← Place Id column, ONLY if it looks like a valid Google Places ID:
  - Valid: starts with "ChI", "Ek", "Ej", "En" and is alphanumeric (length > 10)
  - Invalid: contains commas (it's coordinates), contains spaces/words like "Miss spelled", "Multiple names", "having multiple names"
- `operationalDetails.comingSoon` ← true if Station Status contains "coming soon" (case-insensitive)
- `operationalDetails.underRepair` ← true if Station Status contains "under repair" or "under repiar" (there's a typo in the data)
- `metadata.source` ← "ExcelImport"

**Error Handling — keep it simple:**
- No Drive link in row → skip with warning
- Drive download fails (network error, 404, etc.) → skip with warning, continue next row
- decode() returns null → skip with warning
- DB upsert fails → log error, continue next row
- Always clean up temp Excel file with fs.unlinkSync at the end (in both success and error paths)

**Batch Processing:**
Process in batches of 5 (smaller than the PlugShare import's 10 to avoid hammering Google Drive). Use the same Promise.all batch pattern as uploadPlugShareData() in siteController.js.

**Dependencies to add:**
- `xlsx@0.18.5` — for parsing Excel files (run: pnpm add xlsx@0.18.5)
- `axios` is already installed — use for Drive downloads

**Files to create/modify:**
1. `package.json` — add xlsx dependency
2. `server/helpers/parseExcelSites.js` — NEW helper file with:
   - `parseExcelRows(filePath)` — parse xlsx, return array of row objects
   - `extractDriveFileId(url)` — extract file ID from Drive share link (return null if not a Drive link)
   - `isValidGooglePlaceId(value)` — validate Place ID format
   - `enrichSiteWithExcelRow(decodedSite, excelRow)` — merge Excel metadata onto decoded site
3. `server/controllers/siteController.js` — add `uploadExcelSites` controller function (export it)
4. `server/routes/adminRoutes.js` — add route: `router.post("/upload-excel-sites", upload.single("excelFile"), siteController.uploadExcelSites)`

**Follow these existing patterns:**
- `server/helpers/decodePlugshare.js` — the decode() function you'll import and reuse
- `server/controllers/siteController.js` lines 518-672 — the uploadPlugShareData() function for:
  - Upsert pattern (findOne by metadata.plugShareId → create or findOneAndUpdate)
  - Batch processing (BATCH_SIZE, Promise.all per batch)
  - Import summary format (total, successful, updated, failed, skipped, errors[], warnings[])
  - File cleanup (fs.unlinkSync in try and catch)

**Response format (match existing uploadPlugShareData):**
{
  "message": "Excel import completed",
  "summary": {
    "total": 98,
    "successful": 85,
    "updated": 5,
    "failed": 3,
    "skipped": 5,
    "errors": [...],
    "warnings": [...],
    "totalProcessed": 93,
    "successRate": "91.84%"
  }
}

**Testing:**
The `data_sample/` folder in the project root has:
- `stations data collaction.xlsx` — the actual Excel file (98 rows, cities: Gurgoun, Delhi, Bangaluru)
- 6 sample JSON files — these are PlugShare format for reference (from Chennai, different stations)

**Add Swagger JSDoc** to the controller function.
After implementation, update WORK_STATUS.md with completion status and update docs/api-quick-reference.md.
```

---

### Frontend Dashboard Agent Prompt — Excel Import UI

```
## Task: Add Excel Import UI to Dashboard Sites Section

### Context
The API backend has (or will have) a new endpoint: POST /api/a/upload-excel-sites
This endpoint accepts an Excel (.xlsx) file upload and imports charging stations from it. The server downloads JSON data from Google Drive links in the Excel and imports the stations.

### What to Build

Add an "Import Excel" button to the Sites management section.

**Where to add it:**
Look at how the existing PlugShare import works in the Sites section (check SitesListPage.jsx or wherever the "Import" / "Upload PlugShare" button is). Add an "Import Excel" option alongside it — either as a second button or in a dropdown menu.

**UI Flow:**
1. User clicks "Import Excel" button
2. Dialog/modal opens with:
   - File input accepting `.xlsx` files only
   - Brief description: "Upload an Excel file with station data. Each row should have a Google Drive link to the station's PlugShare JSON."
   - Upload button + Cancel button
3. During upload: show loading spinner (this can take a few minutes — server downloads from Drive per row)
4. After completion: show import summary in the dialog:
   - Total rows
   - Successfully imported (new)
   - Updated (existing)
   - Failed (with expandable error list)
   - Skipped
   - Success rate %
5. On error: show error message with retry option

**API Integration:**
Add to `src/api/sites.js`:
```js
uploadExcelSites: async (file) => {
  const formData = new FormData();
  formData.append('excelFile', file);
  const response = await apiClient.post('/upload-excel-sites', formData, {
    headers: { 'Content-Type': 'multipart/form-data' },
    timeout: 300000, // 5 min timeout — server downloads from Google Drive per row
  });
  return response.data;
},
```

**Endpoint details:**
- URL: POST /api/a/upload-excel-sites
- Auth: Admin JWT (handled by existing axios interceptors)
- Request: multipart/form-data, field name `excelFile`
- Response: { message: string, summary: { total, successful, updated, failed, skipped, errors[], warnings[], totalProcessed, successRate } }

**Follow existing patterns:**
- Use existing shadcn/ui components (Dialog, Button, Input/file)
- Match the existing PlugShare import UI pattern
- Use the same toast/notification approach for feedback
- Keep it consistent with the dashboard design language

After implementation, update WORK_STATUS.md with completion status.
```

---

**End of Agent Communication Prompts**
