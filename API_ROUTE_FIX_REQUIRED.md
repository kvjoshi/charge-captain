# API ROUTE FIX REQUIRED - URGENT

**Date**: 2024-12-19
**From**: Dashboard Instance
**To**: API Instance
**Priority**: 🔥 HIGH - Blocking Dashboard feature

---

## Missing Route: updateSite

The `updateSite` controller EXISTS at line 751 in `siteController.js` but the **route is NOT registered** in `adminRoutes.js`.

### Fix Required

**File**: `chargeCaptainApi/server/routes/adminRoutes.js`

**Add this line after line 38** (after `getSiteById` route):

```javascript
router.put("/updateSite/:id", siteController.updateSite);
```

### Current State (lines 30-38):
```javascript
// site basic controllers
router.post("/createSite", siteController.createSite);
router.post("/getAllSites", siteController.getAllSites);
// New paginated admin sites endpoint
router.get("/sites", getSitesForAdmin);
// New helper endpoints for state/city filtering
router.get("/sites/states", getStates);
router.get("/sites/cities", getCitiesInState);
router.get("/getSiteById/:id", siteController.getSiteById);
// ADD THIS LINE:
// router.put("/updateSite/:id", siteController.updateSite);
```

### After Fix (should look like):
```javascript
// site basic controllers
router.post("/createSite", siteController.createSite);
router.post("/getAllSites", siteController.getAllSites);
// New paginated admin sites endpoint
router.get("/sites", getSitesForAdmin);
// New helper endpoints for state/city filtering
router.get("/sites/states", getStates);
router.get("/sites/cities", getCitiesInState);
router.get("/getSiteById/:id", siteController.getSiteById);
router.put("/updateSite/:id", siteController.updateSite);  // <-- ADD THIS
```

### Expected API Endpoints After Fix

| Method | Endpoint | Controller | Status |
|--------|----------|------------|--------|
| POST | `/api/a/createSite` | createSite | ✅ Working |
| PUT | `/api/a/updateSite/:id` | updateSite | ❌ Route missing |
| GET | `/api/a/getSiteById/:id` | getSiteById | ✅ Working |

### Dashboard is Waiting

Dashboard instance is building the Create/Edit Site UI and needs this route to be registered for the Edit functionality to work.

**Please add this route and update WORK_STATUS.md when done!**
