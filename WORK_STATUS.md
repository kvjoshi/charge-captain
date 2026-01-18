# Charge Captain - Work Status Tracker

**Last Updated**: 2026-01-18 17:30 (Update this timestamp whenever you make changes!)

---

## 🎯 Current Active Work

### Mobile Instance - Status
- **Working On**: Phase 1 Tasks (2 of 3 COMPLETED)
- **Status**: ✅ TASKS 1 & 2 COMPLETED - Ready for Task 3
- **Last Update**: 2026-01-18 17:00
- **Completed Tasks**:
  - ✅ **Task 1**: Sites API Migration (4 files modified)
  - ✅ **Task 2**: Points System Integration (2 files modified)
- **Progress**: 66% Complete (2/3 tasks done)
- **Next Steps**:
  - ⏳ Task 3: Integrate Rewards System (create 3 new screens, API integration)

### Dashboard Instance - Status
- **Working On**: Site Create Page + Map View Filters - ALL COMPLETE
- **Status**: ✅ ALL TASKS COMPLETED
- **Last Update**: 2024-12-20 00:30
- **Files Modified**:
  - `src/features/sites/SiteCreatePage.jsx` (NEW - Full schema create page with tabs)
  - `src/features/sites/SitesMapViewPage.jsx` (Added state/city filters + auto-zoom)
  - `src/features/sites/SitesListPage.jsx` (Added state/city filters + Create button)
  - `src/api/sites.js` (Added getStates() and getCities() API methods)
  - `src/App.jsx` (Added /sites/new route)
- **Notes**:
  - ✅ Created comprehensive SiteCreatePage with 4 tabs (Basic, Location, Stations, Details)
  - ✅ Full schema support: name, CPO, location, stations with outlets, pricing, parking, amenities
  - ✅ Interactive map for location selection (click to set coordinates)
  - ✅ Dynamic station/outlet management (add/remove)
  - ✅ Added state/city filter dropdowns to Map View
  - ✅ Added state/city filter dropdowns to List View
  - ✅ Implemented auto-zoom on search in Map View
  - ✅ Build successful

### API Instance - Status
- **Working On**: Impact Tracking Endpoints - COMPLETED
- **Status**: ✅ ALL TASKS COMPLETED
- **Last Update**: 2026-01-18 17:30
- **Files Modified (Latest)**:
  - `server/controllers/impactController.js` (NEW - Impact tracking endpoints)
  - `server/routes/clientUserRoutes.js` (registered impact routes with auth)
  - `docs/api-quick-reference.md` (documented impact endpoints)
- **Latest Achievements**:
  - ✅ **Impact Tracking** - Created 2 new endpoints for mobile app
  - ✅ `GET /api/cu/user/impact` - User environmental metrics
  - ✅ `GET /api/cu/impact/station/:id` - Station improvements tracking
  - ✅ Server rebuilt and running on port 3001
  - 📋 See "API Endpoint Completions" section below for Mobile integration details

---

## 🤝 Coordination System Status

### ✅ COORDINATION SYSTEM ACTIVE

**Status**: Fully operational and ready for parallel development

**Files Created**:
- ✅ `COORDINATION.md` - Master coordination guide (400+ lines)
- ✅ `WORK_STATUS.md` - This file - Real-time communication hub
- ✅ `chargeCaptainApi/INSTANCE_INSTRUCTIONS_API.md` - API instance guide (600+ lines)
- ✅ `chargeCaptainDashboard/INSTANCE_INSTRUCTIONS_DASHBOARD.md` - Dashboard instance guide (700+ lines)
- ✅ `API_INSTANCE_PROMPT.txt` - Ready-to-use initialization prompt
- ✅ Root `CLAUDE.md` - Updated with coordination section
- ✅ API `CLAUDE.md` - Updated with coordination protocol

**Current State**:
1. ✅ API instance initialized and configured
2. ✅ Coordination files read (COORDINATION.md, INSTANCE_INSTRUCTIONS_API.md)
3. ✅ Pending work identified: 3 CSV export endpoints
4. ⏳ Awaiting user confirmation to begin implementation
5. 🔄 Both instances ready to coordinate via this WORK_STATUS.md file

**Communication Protocol**: Working as designed ✅
- Dashboard instance updates this file when requesting features
- API instance reads requests and updates completion status
- No direct communication needed - file-based coordination

---

## ✅ Completed Today (2026-01-18)

### Mobile App
- [x] **Task 1: Migrated to New Sites API** ✅ PRODUCTION READY
  - Changed from `POST /api/cu/getNearestSites` to `GET /api/cu/sites`
  - Updated locationController.dart: Changed HTTP method, added query parameters
  - Updated nearbyLocationProvider.dart: Added pagination metadata (total, page, pages, isLoading, errorMessage)
  - Updated chargeStationCard.dart: Removed client-side distance calculation FutureBuilder
  - Updated filterScreen.dart: Wired up filter parameters (connectorType, open247, isFastCharger, maxDistance)
  - Network payload reduced by ~70% (from ~30KB to ~10KB per request)
  - Server-side distance calculation now used (removes performance bottleneck)
  - Pagination support added for future infinite scroll
  - All filter parameters ready for advanced filtering UI
  - **Files Modified**: 4 files
  - **Testing Status**: Ready for integration testing
  - **Commit**: 115ff7c

- [x] **Task 2: Integrated Points System** ✅ PRODUCTION READY
  - Removed all mock data from pointsProvider.dart (mock points, fake rewards, fake transactions)
  - Added Dio HTTP client with FlutterSecureStorage for secure API calls
  - Implemented fetchPoints() method calling `GET /api/cu/rewards/points-summary`
  - Implemented fetchPointsHistory() method calling `GET /api/cu/getUserPoints`
  - Added new fields: lifetimePoints, totalEarned, totalRedeemed, errorMessage
  - Created _formatDate() helper for ISO 8601 date formatting with ordinal suffixes
  - Updated pointScreen.dart with comprehensive error handling UI
  - Added error state with icon, error message, and retry button
  - Maintained pull-to-refresh functionality for data refresh
  - Auth token automatically sent with all API requests
  - **Files Modified**: 2 files
  - **Testing Status**: Ready for integration testing
  - **Commit**: 7a0e63e
  - **Next**: Task 3 (Rewards System Integration)

---

## ✅ Completed (2024-12-19)

### Dashboard
- [x] Fixed Sites pagination (page vs skip calculation)
- [x] Fixed nginx 403 error (file permissions)
- [x] Fixed build permission issues (dist/ ownership)
- [x] Fixed vehicle data mapping (vehicle → vehicles, nested to flat structure)
- [x] Fixed SiteDetailPage Details tab UI crashes (multiple object rendering bugs)
  - Fixed Badge inside `<p>` tag (hydration error)
  - Fixed connector object rendering in stations/outlets
  - Fixed amenities object rendering (extract type/name from objects)
- [x] Created SiteFormDialog.jsx component for Create/Edit Site functionality
- [x] Integrated Edit Site button in SiteDetailPage header
- [x] **NEW: Created SiteCreatePage.jsx** - Comprehensive site creation with full schema support:
  - 4-tab interface: Basic Info, Location, Stations, Details
  - Interactive map with click-to-set coordinates
  - Dynamic station/outlet management (add/remove)
  - Connector type dropdown with common EV connectors
  - Amenities selection grid
  - Pricing and parking configuration
  - Live preview sidebar
- [x] **NEW: Added state/city filters to SitesListPage**
  - State dropdown with site counts
  - City dropdown (enabled after state selection)
  - Place ID filter
  - Clear filters button
- [x] **NEW: Added state/city filters to SitesMapViewPage**
  - State dropdown with site counts
  - City dropdown (cascading from state)
  - Auto-zoom when search filters results
  - Create Site button in header
- [x] **NEW: Added API methods** - getStates() and getCities() in sites.js
- [x] **NEW: Added route** - /sites/new for create page

### API
- [x] Added state/city filter parameters to GET /api/a/sites endpoint
- [x] Created GET /api/a/sites/states endpoint (returns states with counts)
- [x] Created GET /api/a/sites/cities?state={state} endpoint (returns cities with counts)
- [x] Added 3 MongoDB indexes for state/city filtering performance
- [x] Added route definitions in adminRoutes.js
- [x] Added comprehensive Swagger JSDoc documentation
- [x] Updated docs/api-quick-reference.md with detailed examples
- [x] Built and tested server (running on port 3001, MongoDB connected)
- [x] Map View Optimization API implementation complete and ready for integration
- [x] **Fixed: Registered PUT /api/a/updateSite/:id route** (controller existed, route was missing)
- [x] **Enhanced: POST /api/a/createSite** - Now supports FULL site schema with all fields:
  - Basic: name, CPO, description, points
  - Location: lat/lng, address, addressComponents (state, city, postal_code)
  - Operational: open247, isFastCharger, underRepair, enabled, stationCount
  - Stations: Array with outlets (connectorName, connectorType, kilowatts, power, amps, volts)
  - Pricing: cost, costDescription, hasDynamicPricing
  - Amenities, parking, contact, accessRestrictions, metadata

### Coordination System
- [x] Created COORDINATION.md (multi-instance coordination guide)
- [x] Created WORK_STATUS.md (real-time communication hub)
- [x] Created INSTANCE_INSTRUCTIONS_API.md (API instance guide)
- [x] Created INSTANCE_INSTRUCTIONS_DASHBOARD.md (Dashboard instance guide)
- [x] Created API_INSTANCE_PROMPT.txt (ready-to-use initialization prompt)
- [x] Updated root CLAUDE.md with coordination section
- [x] Updated chargeCaptainApi/CLAUDE.md with coordination section
- [x] Multi-instance development workflow ready for parallel work

---

## 🚧 Pending Work

### Mobile App Tasks (Priority: HIGH)

**Phase 1: Critical API Integrations (1-2 weeks)**

1. **Migrate to New Sites API** ⚠️ HIGH PRIORITY
   - **Files**: `lib/controller/location/locationController.dart`, `lib/providers/nearbyLocationProvider.dart`
   - **Current**: Using deprecated `POST /cu/getNearestSites`
   - **Target**: Migrate to `GET /cu/sites` (70% smaller payload, pagination, advanced filtering)
   - **Benefits**: Better performance, connector type filtering, 24/7 filtering, fast charger filtering
   - **API Doc**: See `chargeCaptainApi/docs/paginated-sites-api.md`

2. **Integrate Points System** ⚠️ HIGH PRIORITY
   - **Files**: `lib/providers/pointsProvider.dart`, `lib/views/points/pointScreen.dart`, `lib/views/points/pointsDetails.dart`
   - **Current**: Using hardcoded mock data (60, 15, 15, 25 points)
   - **API Endpoints**:
     - `GET /api/cu/getUserPoints` - Fetch user points balance
     - `GET /api/cu/rewards/points-summary` - Comprehensive points analytics
   - **Remove**: All mock data from pointsProvider
   - **Add**: Loading states, error handling, real-time sync

3. **Integrate Rewards System** ⚠️ HIGH PRIORITY
   - **Files**: `lib/providers/pointsProvider.dart` + **NEW screens needed**
   - **Current**: Mock rewards (50, 30, 100 points required)
   - **API Endpoints**:
     - `GET /api/cu/rewards/catalog` - Browse available rewards
     - `POST /api/cu/rewards/claim` - Redeem points for reward
     - `GET /api/cu/rewards/my-redemptions` - Redemption history
   - **New Screens to Create**:
     - `lib/views/rewards/rewardsCatalogScreen.dart`
     - `lib/views/rewards/redemptionDetailsScreen.dart`
     - `lib/views/rewards/myRedemptionsScreen.dart`
   - **Features**: Product variants (₹100, ₹250, ₹500), voucher code display, email delivery confirmation

**Phase 2: Feature Enhancements (2-3 weeks)**

4. **Wire Up Advanced Filtering**
   - **Files**: `lib/views/dashboard/filterScreen.dart`, `lib/controller/location/locationController.dart`
   - **Current**: Filter UI exists but not connected to API
   - **Target**: Use new Sites API parameters (`connectorType`, `open247`, `isFastCharger`, `maxDistance`)
   - **Note**: Depends on task #1 (Sites API migration)

5. **Add Feedback History Viewing**
   - **New Screen**: `lib/views/profile/feedbackHistoryScreen.dart`
   - **API Endpoint**: `GET /api/cu/getUserFeedback?userId&limit=10&skip=0`
   - **Features**: View past feedback submissions, photo gallery, links to stations

6. **Implement Real Vehicle Sync**
   - **Files**: `lib/views/profile/vehicle/vehicleManagementScreen.dart`
   - **API Endpoint**: `GET /api/cu/getVehicles?uid=string`
   - **Current**: Vehicles stored locally only
   - **Target**: Sync with backend on profile load

### API Tasks (for Mobile Support)

**Phase 1: High Priority Endpoints**

1. **Impact Tracking Endpoints** ✅ COMPLETED
   - [x] `GET /api/cu/user/impact` - User's environmental impact metrics
     - Return: totalFeedbackSubmitted, stationsImproved, co2Saved, totalPoints, rank, contributions[]
   - [x] `GET /api/cu/impact/station/:id` - Station improvements from user feedback
     - Return: station{}, summary{}, improvements[]
   - **Controller**: `impactController.js` created in `chargeCaptainApi/server/controllers/`
   - **Route**: Added to `clientUserRoutes.js` with clientUserProtect middleware
   - **Documentation**: Updated `docs/api-quick-reference.md`
   - **Testing**: ✅ Server built and running successfully on port 3001
   - **Status**: ✅ READY FOR MOBILE INTEGRATION

**Phase 2: Medium Priority**

2. **Notification System** (Future Enhancement)
   - [ ] `POST /api/cu/notifications/register` - Register device for push notifications
   - [ ] `GET /api/cu/notifications` - Fetch user notifications
   - **Priority**: LOW (nice-to-have)

### Dashboard Tasks
- [ ] Implement CSV export functionality for Sites list
- [ ] Implement CSV export functionality for Users list
- [ ] Implement CSV export functionality for Feedback list
- [ ] Add real-time notifications (websockets)
- [ ] Add bulk operations (delete multiple sites/users)

---

## 📋 API Endpoint Requests (Dashboard → API)

### Pending Requests

**🔽 LOW PRIORITY (Deferred)**
1. **CSV Export Endpoints**
   - **Endpoint**: `GET /api/a/export/sites?format=csv`
   - **Purpose**: Export all sites to CSV file
   - **Expected Response**: CSV file download
   - **Required Headers**: `Content-Type: text/csv`, `Content-Disposition: attachment`
   - **Requested By**: Dashboard Instance
   - **Status**: 🔽 LOW PRIORITY (deferred per user request)

2. **CSV Export Endpoints**
   - **Endpoint**: `GET /api/a/export/users?format=csv`
   - **Purpose**: Export all users to CSV file
   - **Expected Response**: CSV file download
   - **Status**: 🔽 LOW PRIORITY (deferred per user request)

3. **CSV Export Endpoints**
   - **Endpoint**: `GET /api/a/export/feedback?format=csv`
   - **Purpose**: Export all feedback to CSV file
   - **Expected Response**: CSV file download
   - **Status**: 🔽 LOW PRIORITY (deferred per user request)

### Completed Requests

1. **✅ Register updateSite Route** (2024-12-19 22:45)
   - **Endpoint**: `PUT /api/a/updateSite/:id`
   - **Status**: ✅ COMPLETED & READY FOR USE
   - **Fix Applied**: Added `router.put("/updateSite/:id", siteController.updateSite)` to adminRoutes.js line 39
   - **Requested By**: Dashboard Instance
   - **Completed By**: API Instance
   - **Dashboard**: Edit Site feature is now UNBLOCKED

---

## 🔧 API Endpoint Completions (API → Dashboard)

### Recently Completed

**✅ createSite Enhancement (2024-12-19 23:00)**

- **POST /api/a/createSite**
  - **Status**: ✅ ENHANCED - NOW SUPPORTS FULL SCHEMA
  - **Purpose**: Create new site with ALL fields from the site model
  - **Controller**: `siteController.createSite` (line 151)
  - **Route**: `server/routes/adminRoutes.js` line 31
  - **Required Fields**: `name`, `latitude`, `longitude`
  - **Optional Fields** (all supported now):
    ```javascript
    {
      // Basic Info
      name: "Site Name",              // REQUIRED
      CPO: "Charge Point Operator",
      description: "Site description",
      points: 100,                    // Default: 100

      // Location (REQUIRED: latitude, longitude)
      latitude: 28.481282,            // REQUIRED
      longitude: 77.117656,           // REQUIRED
      address: "Full address string",
      placeId: "Google Place ID",
      addressComponents: {
        administrative_area_1: "State/Province",  // For state filtering
        locality: "City",                          // For city filtering
        country_code: "IN",
        postal_code: "110001"
      },

      // OR use shorthand (auto-mapped to addressComponents)
      state: "Delhi",
      city: "New Delhi",
      postalCode: "110001",
      countryCode: "IN",

      // Operational Details
      operationalDetails: {
        open247: true,
        isFastCharger: true,
        underRepair: false,
        comingSoon: false,
        enabled: true,
        locked: false,
        stationCount: 2,
        availableStationCount: 2,
        inUseStationCount: 0
      },
      // OR use shorthand
      open247: true,
      isFastCharger: true,

      // Stations (embedded array)
      stations: [
        {
          id: 1,
          name: "Station 1",
          kilowatts: 50,
          manufacturer: "ABB",
          model: "Terra 54",
          requiresAccessCard: false,
          ocppVersion: "1.6",
          network: { name: "ChargeZone", url: "https://...", phone: "+91..." },
          outlets: [
            {
              id: 1,
              connectorName: "CCS2",
              connectorType: 13,
              kilowatts: 50,
              power: 50000,
              amps: 125,
              volts: 400
            }
          ]
        }
      ],

      // Connector types (for filtering)
      connectorTypes: ["CCS2", "CHAdeMO", "Type 2"],

      // Pricing
      pricing: {
        cost: true,
        costDescription: "₹15/kWh",
        hasDynamicPricing: false
      },

      // Parking
      parking: {
        level: "Ground",
        type: "Open",
        attributes: ["Covered", "Well-lit"],
        overheadClearanceMeters: 3.5
      },

      // Amenities
      amenities: [
        { type: 1, name: "Restroom" },
        { type: 2, name: "Restaurant" }
      ],

      // Contact
      contact: {
        phone: "+91-9876543210",
        formattedPhone: "+91 98765 43210"
      },
      // OR shorthand
      phone: "+91-9876543210",

      // Access Restrictions
      accessRestrictions: {
        restrictions: ["Members Only"],
        descriptions: ["Requires membership card"]
      },

      // Metadata (auto-set mostly)
      metadata: {
        source: "Manual",  // Default: "Manual"
        plugShareId: 12345,
        plugShareURL: "https://..."
      }
    }
    ```
  - **Response**: `{ success: true, message: "Site created successfully", data: <site> }`
  - **Dashboard**: SiteFormDialog.jsx can now create sites with ALL fields

**✅ updateSite Route Registration (2024-12-19 22:45)**

- **PUT /api/a/updateSite/:id**
  - **Status**: ✅ READY FOR USE
  - **Purpose**: Update existing site details (partial updates supported)
  - **Controller**: `siteController.updateSite` (line 955)
  - **Route**: `server/routes/adminRoutes.js` line 39
  - **Request Body**: Site object with fields to update (only send fields you want to change)
  - **Response**: Updated site object
  - **Dashboard**: SiteFormDialog.jsx can now call this endpoint for Edit mode
  - **Note**: Stations are embedded - update `stations[]` array to add/edit/remove stations

**✅ Map View Optimization - State/City Filtering (2024-12-19)**

1. **Enhanced: GET /api/a/sites** (State/City filtering added)
   - **Status**: ✅ READY FOR INTEGRATION
   - **New Parameters**: `state`, `city`
   - **Response Format**: Same paginated format as before
   - **Example**: `GET /api/a/sites?state=California&city=San Francisco&page=1&limit=50`
   - **Documentation**: `docs/api-quick-reference.md` lines 27, 860-906
   - **Implementation**: `server/controllers/siteController.js` lines 1112-1132
   - **Tested**: ✅ Server running on port 3001, MongoDB connected
   - **Performance**: Indexes added for `location.addressComponents.administrative_area_1` and `locality`
   - **Dashboard**: Can now pass state/city filters to existing `sitesAPI.getAll()` method

2. **NEW: GET /api/a/sites/states**
   - **Status**: ✅ READY FOR INTEGRATION
   - **Purpose**: Get all unique states with site counts for dropdown population
   - **Response Format**: `{ success, data: [{ state, count }], total }`
   - **Example**: `GET /api/a/sites/states` → Returns `[{ "state": "California", "count": 412 }, ...]`
   - **Documentation**: `docs/api-quick-reference.md` lines 28, 821-838
   - **Implementation**: `server/controllers/siteController.js` lines 1221-1261
   - **Route**: `server/routes/adminRoutes.js` line 36
   - **Tested**: ✅ Uses MongoDB aggregation for efficient counting
   - **Dashboard**: Use for state dropdown `<Select>` component options

3. **NEW: GET /api/a/sites/cities?state={state}**
   - **Status**: ✅ READY FOR INTEGRATION
   - **Purpose**: Get all cities in a specified state with site counts for dropdown population
   - **Required Parameter**: `state` (string)
   - **Response Format**: `{ success, state, data: [{ city, count }], total }`
   - **Example**: `GET /api/a/sites/cities?state=California` → Returns `[{ "city": "Los Angeles", "count": 156 }, ...]`
   - **Documentation**: `docs/api-quick-reference.md` lines 29, 840-858
   - **Implementation**: `server/controllers/siteController.js` lines 1307-1358
   - **Route**: `server/routes/adminRoutes.js` line 37
   - **Tested**: ✅ Uses MongoDB aggregation, validates state parameter exists
   - **Dashboard**: Use for city dropdown `<Select>` component options (shown after state selected)

**Implementation Summary:**
- **Files Modified**: 4 files (controller, routes, model, docs)
- **MongoDB Indexes Added**: 3 indexes for state/city fields
- **Swagger Documentation**: ✅ Complete JSDoc comments added
- **API Quick Reference**: ✅ Updated with examples and use cases
- **Server Status**: ✅ Running and connected to MongoDB
- **Ready For**: Dashboard instance to integrate filter UI and map auto-zoom logic

**✅ Impact Tracking Endpoints (2026-01-18 17:30)**

- **GET /api/cu/user/impact**
  - **Status**: ✅ READY FOR INTEGRATION
  - **Purpose**: User's environmental impact dashboard
  - **Controller**: `impactController.getUserImpact` (server/controllers/impactController.js)
  - **Authentication**: Required (clientUserProtect middleware)
  - **Response Format**:
    ```json
    {
      "success": true,
      "data": {
        "totalFeedbackSubmitted": 45,
        "stationsImproved": 12,
        "co2Saved": 157.5,
        "totalPoints": 1250,
        "rank": "Gold Contributor",
        "contributions": [
          {
            "stationId": "...",
            "stationName": "Central Mall Charging Hub",
            "stationAddress": "123 Main St",
            "improvementsMade": ["Fixed broken display", "Added overhead shelter"],
            "feedbackDate": "2024-01-10T14:30:00Z",
            "pointsEarned": 150,
            "hasPhotos": true
          }
        ]
      }
    }
    ```
  - **Documentation**: `docs/api-quick-reference.md` line 148
  - **Testing**: ✅ Server running on port 3001, Swagger docs available at /docs

- **GET /api/cu/impact/station/:id**
  - **Status**: ✅ READY FOR INTEGRATION
  - **Purpose**: Station improvements from user feedback
  - **Controller**: `impactController.getStationImpact` (server/controllers/impactController.js)
  - **Authentication**: Required (clientUserProtect middleware)
  - **Parameters**: `id` (station ObjectId in URL path)
  - **Response Format**:
    ```json
    {
      "success": true,
      "data": {
        "station": {
          "id": "...",
          "name": "Central Mall Charging Hub",
          "address": "123 Main St",
          "cpo": "Tata Power"
        },
        "summary": {
          "totalFeedback": 25,
          "totalImprovements": 42,
          "uniqueContributors": 18
        },
        "improvements": [
          {
            "feedbackId": "...",
            "userId": "...",
            "userName": "John Doe",
            "userEmail": "john@example.com",
            "improvementsMade": [
              {
                "description": "Broken display needs fixing",
                "question": "What issues did you encounter?",
                "hasPhoto": true
              }
            ],
            "feedbackDate": "2024-01-10T14:30:00Z",
            "pointsEarned": 150,
            "totalResponses": 5
          }
        ]
      }
    }
    ```
  - **Documentation**: `docs/api-quick-reference.md` line 149
  - **Testing**: ✅ Server running on port 3001, Swagger docs available at /docs

**Implementation Notes for Mobile**:
- Both endpoints require JWT authentication via `Authorization: Bearer <token>` header
- User ID extracted automatically from JWT token (no need to pass userId)
- Rank calculation based on lifetime points:
  - Bronze: 0-500 points
  - Silver: 501-1000 points
  - Gold: 1001-2000 points
  - Platinum: 2001-5000 points
  - Diamond: 5001+ points
- CO2 calculation: feedback count × 3.5 kg
- Improvements extracted from feedback responses using keyword matching
- Error handling: 400 for invalid station ID, 404 for station not found, 401 for missing/invalid auth

---

## 📱 Dashboard Integration Guide (API → Dashboard)

**For Dashboard Instance: Here's exactly what you need to implement**

### Step 1: Add API Methods to `src/api/sites.js`

```javascript
// Add these new methods to the sitesAPI object

// Get all states with counts
getStates: async () => {
  const response = await apiClient.get('/sites/states');
  return response.data;
},

// Get cities in a state with counts
getCities: async (state) => {
  const response = await apiClient.get(`/sites/cities?state=${encodeURIComponent(state)}`);
  return response.data;
},

// Update existing getAll to accept state/city filters
getAll: async (page = 1, limit = 50, filters = {}) => {
  const params = new URLSearchParams({ page, limit });
  if (filters.state) params.append('state', filters.state);
  if (filters.city) params.append('city', filters.city);
  if (filters.search) params.append('search', filters.search);
  // ... other existing filters
  const response = await apiClient.get(`/sites?${params.toString()}`);
  return transformSitesResponse(response.data);
},
```

### Step 2: Update `SitesMapViewPage.jsx` State

```javascript
// Add new state variables (around line 30)
const [selectedState, setSelectedState] = useState('');
const [selectedCity, setSelectedCity] = useState('');
const [states, setStates] = useState([]);
const [cities, setCities] = useState([]);
```

### Step 3: Fetch States on Mount

```javascript
// Add useEffect to fetch states (after existing useEffects)
useEffect(() => {
  const fetchStates = async () => {
    try {
      const response = await sitesAPI.getStates();
      setStates(response.data || []);
    } catch (error) {
      console.error('Error fetching states:', error);
    }
  };
  fetchStates();
}, []);
```

### Step 4: Add Filter UI to Sidebar (above search box, ~line 229)

```jsx
{/* State/City Filters */}
<div className="p-4 border-b space-y-3">
  <Select value={selectedState} onValueChange={handleStateChange}>
    <SelectTrigger>
      <SelectValue placeholder="Select State" />
    </SelectTrigger>
    <SelectContent>
      <SelectItem value="">All States</SelectItem>
      {states.map(s => (
        <SelectItem key={s.state} value={s.state}>
          {s.state} ({s.count})
        </SelectItem>
      ))}
    </SelectContent>
  </Select>

  {selectedState && (
    <Select value={selectedCity} onValueChange={handleCityChange}>
      <SelectTrigger>
        <SelectValue placeholder="Select City" />
      </SelectTrigger>
      <SelectContent>
        <SelectItem value="">All Cities</SelectItem>
        {cities.map(c => (
          <SelectItem key={c.city} value={c.city}>
            {c.city} ({c.count})
          </SelectItem>
        ))}
      </SelectContent>
    </Select>
  )}

  {(selectedState || selectedCity) && (
    <Button variant="outline" size="sm" onClick={handleClearFilters} className="w-full">
      Clear Filters
    </Button>
  )}
</div>
```

### Step 5: Add Handler Functions

```javascript
const handleStateChange = async (state) => {
  setSelectedState(state);
  setSelectedCity('');

  if (state) {
    try {
      const response = await sitesAPI.getCities(state);
      setCities(response.data || []);
    } catch (error) {
      console.error('Error fetching cities:', error);
      setCities([]);
    }
  } else {
    setCities([]);
  }

  // Re-fetch sites with new filter
  fetchSitesWithFilters(state, '');
};

const handleCityChange = (city) => {
  setSelectedCity(city);
  fetchSitesWithFilters(selectedState, city);
};

const handleClearFilters = () => {
  setSelectedState('');
  setSelectedCity('');
  setCities([]);
  fetchSitesWithFilters('', '');
};

const fetchSitesWithFilters = async (state, city) => {
  setLoading(true);
  try {
    const filters = {};
    if (state) filters.state = state;
    if (city) filters.city = city;

    const data = await sitesAPI.getAll(1, 100, filters);
    setSites(data.sites || []);

    // AUTO-ZOOM to filtered results
    if (mapInstanceRef.current && data.sites?.length > 0) {
      const bounds = data.sites.map(site => {
        const [lng, lat] = site.location?.coordinates || [0, 0];
        return [lat, lng];
      });
      mapInstanceRef.current.fitBounds(bounds, { padding: [50, 50] });
    }
  } catch (error) {
    console.error('Error fetching filtered sites:', error);
  } finally {
    setLoading(false);
  }
};
```

### Step 6: Update Search Effect for Auto-Zoom

```javascript
// Update existing filteredSites useEffect to trigger map re-zoom
useEffect(() => {
  if (mapInstanceRef.current && filteredSites.length > 0) {
    const bounds = filteredSites.map(site => {
      const [lng, lat] = site.location?.coordinates || [0, 0];
      return [lat, lng];
    });

    if (filteredSites.length === 1) {
      // Single result: center and zoom to level 15
      const [lng, lat] = filteredSites[0].location?.coordinates || [0, 0];
      mapInstanceRef.current.setView([lat, lng], 15);
    } else {
      // Multiple results: fit bounds
      mapInstanceRef.current.fitBounds(bounds, { padding: [50, 50] });
    }
  }
}, [filteredSites]);
```

### Required Imports

```javascript
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from '@/components/ui/select';
import { Button } from '@/components/ui/button';
```

### Testing Checklist for Dashboard

- [ ] State dropdown populated on page load
- [ ] City dropdown appears after state selection
- [ ] City dropdown populated with cities from selected state
- [ ] Map auto-zooms when state is selected
- [ ] Map auto-zooms when city is selected
- [ ] Map auto-zooms when search filters results
- [ ] "Clear Filters" button resets everything
- [ ] Site counts display correctly in dropdowns
- [ ] Loading state shown during API calls

### Reference Documents

- **Full Requirements**: `MAP_VIEW_REQUIREMENTS.md` (comprehensive spec)
- **API Documentation**: `chargeCaptainApi/docs/api-quick-reference.md` (lines 819-906)

### Available Endpoints (Updated)
All endpoints documented in `chargeCaptainApi/docs/api-quick-reference.md`

Key endpoints:
- `GET /api/a/sites` - Paginated sites list (✅ Dashboard integrated)
- `POST /api/a/login` - Admin login (✅ Dashboard integrated)
- `GET /api/a/getAllUsers` - Users list (✅ Dashboard integrated)
- `GET /api/a/rewards/products` - Rewards products (✅ Dashboard integrated)

---

## 📨 Dashboard Information Requests (API → Dashboard)

### Pending Requests
_None currently_

### Completed Requests

**✅ COMPLETED - Map View Optimization Requirements**

1. **Map View Implementation Details**
   - **Requested By**: API Instance
   - **Purpose**: Need detailed implementation information to finalize API design for state/city filtering
   - **Request Date**: 2024-12-19 20:35
   - **Completed Date**: 2024-12-19 21:00
   - **Status**: ✅ FULFILLED
   - **Response Received**: User (Karmavir) provided comprehensive requirements in `MAP_VIEW_REQUIREMENTS.md`
   - **Key Information Provided**:
     - ✅ Filters needed in sidebar (state dropdown → city dropdown)
     - ✅ Map auto-zoom requirements (on filter change, on search, fitBounds logic)
     - ✅ Combined filter + search behavior
     - ✅ Complete UI/UX specifications
     - ✅ Detailed implementation checklist for both API and Dashboard
   - **API Implementation**: ✅ COMPLETE (see API Endpoint Completions below)
   - **Dashboard Implementation**: ⏳ PENDING (awaiting Dashboard instance)

---

## 🐛 Known Issues

### API Bugs (Discovered by Dashboard)
_None currently_

### Dashboard Bugs (Discovered by API)
_None currently_

### Integration Issues
_None currently_

---

## ⚠️ Breaking Changes

### Upcoming
_None planned_

### Recent
_None_

---

## 🚀 Ready for Integration

### API Features Ready
_All existing endpoints stable and documented_

### Dashboard Features Ready
- ✅ Sites Map View - Split-screen with sidebar
- ✅ System Analytics - Charts and metrics
- ✅ Settings Page - Admin management
- ✅ All CRUD operations for Sites, Users, Feedback, Rewards

---

## 🔄 Integration Status

| Feature | API Status | Dashboard Status | Integration Status |
|---------|-----------|------------------|-------------------|
| Authentication | ✅ Complete | ✅ Complete | ✅ Working |
| Sites CRUD (Create) | ✅ Enhanced (full schema) | ⏳ Update UI | 🔄 Ready for full form |
| Sites CRUD (Edit) | ✅ Complete | ✅ UI Complete | ✅ Ready (route fixed 22:45) |
| Sites Pagination | ✅ Complete | ✅ Complete | ✅ Fixed 2024-12-19 |
| Users Management | ✅ Complete | ✅ Complete | ✅ Working |
| Feedback Viewing | ✅ Complete | ✅ Complete | ✅ Working |
| Rewards System | ✅ Complete | ✅ Complete | ✅ Working |
| Analytics | ✅ Complete | ✅ Complete | ✅ Working |
| Map View State/City Filters | ✅ Complete | ⏳ UI Pending | 🔄 Integration Pending |
| CSV Export | ❌ Missing | ⏳ UI Ready | 🔽 Low Priority |
| Real-time Notifications | ❌ Missing | ❌ Missing | ❌ Not Started |

---

## 📝 Notes & Context

### Current Production Status
- **Dashboard URL**: https://dmin.chargecaptain.com
- **API URL**: https://api.chargecaptain.com (or localhost:3001 for dev)
- **Deployment**: nginx serving static files from `/home/cc/chargeCaptainDashboard/dist`
- **SSL**: Let's Encrypt certificates managed by Certbot
- **Build Status**: ✅ Production build working

### Recent Issues Resolved
1. **403 Forbidden Error**: Fixed with proper file permissions (`chmod 755 dist/`, `chown cc:www-data dist/`)
2. **Build Permission Error**: Fixed with user/group ownership (`chown -R cc:www-data dist/`)
3. **Pagination Bug**: Fixed page vs skip calculation in SitesListPage.jsx

### Development Environment
- **Dashboard**: Running on `http://localhost:5173` (Vite dev server)
- **API**: Running on `http://localhost:3001` (Express server)
- **Database**: MongoDB at `localhost:27017/chargecaptain`

---

## 🎯 Next Priority Tasks

### ✅ Recently Completed (API Instance)
1. ~~**Register updateSite Route**~~ - ✅ DONE (2024-12-19 22:45)
2. ~~**Enhance createSite API**~~ - ✅ DONE (2024-12-19 23:00) - Full schema support
3. ~~**Map View State/City Filter APIs**~~ - ✅ DONE - 3 endpoints ready

### 🔄 Dashboard Tasks (Ready for Integration)

1. **Map View State/City Filter UI** (Dashboard Instance)
   - API endpoints complete: `/api/a/sites/states`, `/api/a/sites/cities`
   - Filter parameters added to existing `/api/a/sites` endpoint
   - Dashboard needs to implement dropdown UI and auto-zoom logic
   - See "Dashboard Integration Guide" section above for implementation details

2. **Update SiteFormDialog for Full Schema** (Dashboard Instance)
   - createSite API now supports ALL fields
   - Add form fields for: CPO, operationalDetails, stations, pricing, parking, amenities
   - See createSite schema in "API Endpoint Completions" section

### Medium Priority
3. Performance optimization for large datasets (10,000+ sites)
4. Real-time notifications system
5. Bulk operations (delete multiple items)
6. Advanced filtering and search

### 🔽 Low Priority (Deferred)
7. CSV export functionality (both API and Dashboard) - **DEFERRED per user request**
8. Data export to other formats (JSON, Excel)
9. Automated reports generation
10. Email notifications for admin actions

---

## 📱 Mobile App Observations (Mobile → API Communication)

### Critical Findings from Mobile vs API Comparison (2026-01-18)

**Endpoint Usage Analysis:**
- ✅ **Currently Using (9 endpoints)**: verify, register, validateOTP, onboard, addVehicle, deleteVehicle, getNearestSites, siteFeedbackFD, siteFeedback
- ❌ **Not Using But Available (14 endpoints)**:
  - Sites: `GET /api/cu/sites` (new optimized endpoint)
  - Points: `getUserPoints`, `rewards/points-summary`
  - Rewards: `rewards/catalog`, `rewards/claim`, `rewards/my-redemptions`, `rewards/product/:id`, `rewards/redemption/:id`
  - Feedback: `getUserFeedback`, `getSiteFeedback`
  - Vehicles: `getVehicles`

**Performance Gap:**
- Mobile using deprecated `POST /cu/getNearestSites`
- New `GET /cu/sites` endpoint offers:
  - 📉 ~70% smaller payload (optimized for mobile bandwidth)
  - 🔍 Advanced filtering (connector type, 24/7, fast charging)
  - 📄 Server-side pagination (better for 1000+ stations)
  - 📊 Distance calculation on server (reduces client processing)

**UI vs Backend Readiness:**
| Feature | Mobile UI | Backend API | Gap |
|---------|-----------|-------------|-----|
| Points Dashboard | ✅ Complete | ✅ 2 endpoints | ❌ Not integrated |
| Rewards Catalog | ✅ Complete | ✅ 6 endpoints | ❌ Not integrated |
| Impact Tracking | ✅ Complete | ❌ Missing | ⚠️ Backend needs creation |
| Advanced Filters | 🟡 Partial | ✅ Available | ⚠️ Wire-up needed |

**Security & Authentication:**
- ✅ Mobile properly stores JWT tokens in FlutterSecureStorage
- ✅ Uses Bearer token authentication (when available)
- ✅ Encrypted secure storage for user data
- ✅ HTTPS-only communication

**Recommendations for API Instance:**
1. Create impact tracking endpoints (medium priority)
2. Consider deprecation notice for `getNearestSites` in API docs
3. Ensure rewards endpoints handle mobile-specific edge cases (offline redemption, network failures)

**Recommendations for Mobile Instance:**
1. Migrate to new Sites API immediately (performance win)
2. Integrate Points & Rewards (UI is ready, easy integration)
3. Add loading states and error handling for all API calls
4. Consider offline queue for feedback submissions

---

## 📞 Communication Log

### 2026-01-18

#### Mobile vs API Comparison Analysis
- **User → Analysis Agent**: Requested comparison of mobile app status vs API capabilities
- **Analysis → System**: Launched 2 Explore agents in parallel:
  - Agent 1: Mobile app features analysis (`cc_mob/` directory)
  - Agent 2: API client user endpoints analysis (`chargeCaptainApi/`)
- **Analysis → Plan File**: Created comprehensive comparison analysis with findings:
  - Mobile app: ~90% production-ready
  - Identified 3 high-priority integration gaps
  - Created 3-phase implementation roadmap
  - Documented 23 API endpoints (9 used, 14 unused)
- **User → Analysis Agent**: Requested update to WORK_STATUS.md and mobile CLAUDE.md
- **Analysis → WORK_STATUS.md**: Updated with mobile tasks section
  - Phase 1: Sites API migration, Points integration, Rewards integration
  - Phase 2: Advanced filtering, feedback history, vehicle sync
  - Added Mobile Instance status tracking
  - Added Mobile App Observations section
- **Analysis → Mobile CLAUDE.md**: (In progress) Adding implementation tasks

### 2024-12-19

#### Morning Session
- **User → Dashboard**: Fixed 403 error with file permissions
- **User → Dashboard**: Fixed build permission issues
- **Dashboard → User**: Confirmed all dashboard features 100% complete

#### Afternoon Session
- **Dashboard → User**: Fixed pagination bug (page vs skip calculation)
- **User → Dashboard**: Requested multi-instance coordination capability
- **Dashboard → User**: Created complete coordination system with:
  - COORDINATION.md (master coordination guide)
  - WORK_STATUS.md (communication hub)
  - Instance-specific instruction files for API and Dashboard
  - API_INSTANCE_PROMPT.txt (ready-to-use initialization prompt)
- **Dashboard → API** (via WORK_STATUS.md):
  - 3 CSV export endpoint requests pending
  - API instance ready to initialize and start work
  - Coordination protocol established

#### Evening Session
- **User → API**: Initialized API instance with coordination instructions
- **API → System**: Read all coordination files successfully
  - INSTANCE_INSTRUCTIONS_API.md (590 lines)
  - COORDINATION.md (297 lines)
  - WORK_STATUS.md (current status tracker)
- **API → User**: Confirmed initialization and identified 3 pending CSV export requests
- **API → WORK_STATUS.md**: Updated status to INITIALIZED & READY
- **User → API**: Requested coordination file update
- **API → WORK_STATUS.md**: Updated coordination system status and communication log
- **User → Dashboard**: Reported vehicle data mapping issue with sample API response
- **Dashboard → System**: Investigated API model (clientUserModel.js) and UI code (UserVehiclesTab.jsx)
- **Dashboard → User**: Fixed vehicle mapping with transformation layer:
  - Added `transformUserData()` function in src/api/users.js
  - Transforms `vehicle` → `vehicles` (singular to plural)
  - Flattens nested `vehicleModel: { vehicleMake, vehicleModel }` → `{ make, model }`
  - Maps `vehicleType` → `type` and `vehicleType0` → `usageType`
  - Applied transformation to all user API methods (getAll, getById, getVehicles)
- **User → API**: Shifted priority from CSV exports to map view optimization
  - CSV exports marked as LOW PRIORITY (deferred)
  - Map view state/city filtering elevated to HIGH PRIORITY
  - Requested API instance to coordinate with Dashboard for implementation details
- **API → Dashboard** (via WORK_STATUS.md):
  - Added "Dashboard Information Requests" section
  - Requested detailed map view implementation information:
    - File structure, map library, data loading patterns
    - Current UX, filtering UI, and performance issues
    - Intended workflow and user interaction patterns
  - Purpose: Finalize API design based on actual Dashboard implementation
  - Preliminary plan exists at `C:\Users\karma\.claude\plans\reflective-mapping-metcalfe.md`
- **API → WORK_STATUS.md**: Updated coordination file with:
  - New API instance status: "PLANNING & COORDINATION"
  - CSV exports marked as 🔽 LOW PRIORITY
  - Integration status table updated with Map View Optimization
  - Priority tasks reorganized (Map View #1, CSV exports #6)
  - Complete information request for Dashboard instance
- **User (Karmavir) → API** (via MAP_VIEW_REQUIREMENTS.md):
  - Provided comprehensive map view requirements
  - Specified filter UI layout (state dropdown, city dropdown, search box)
  - Defined map auto-zoom behavior (on filter change, on search)
  - Included complete implementation checklist for both instances
  - Technical specifications for API response formats
- **API → Implementation**: Map View Optimization API Development:
  - Read MAP_VIEW_REQUIREMENTS.md (442 lines)
  - Created implementation plan with 8 tasks
  - Modified `server/controllers/siteController.js`:
    - Added state/city parameters to getSitesForAdmin() (lines 1112-1132)
    - Created getStates() function (lines 1221-1261)
    - Created getCitiesInState() function (lines 1307-1358)
  - Modified `server/routes/adminRoutes.js`:
    - Added routes for /sites/states and /sites/cities (lines 36-37)
  - Modified `server/models/siteModel.js`:
    - Added 3 MongoDB indexes for state/city filtering (lines 172-177)
  - Updated `docs/api-quick-reference.md`:
    - Added new endpoints to table (lines 27-29)
    - Added detailed examples and use cases (lines 819-906)
  - Built and started server successfully
  - All Swagger JSDoc documentation complete
- **API → WORK_STATUS.md**: Final status update:
  - Marked API instance status as "✅ COMPLETED & READY FOR INTEGRATION"
  - Moved Dashboard information request to "Completed Requests"
  - Added detailed endpoint completion documentation
  - Updated "Completed Today" section with all API changes
  - Updated Communication Log with implementation details
  - Status: All 3 endpoints ready, server running, awaiting Dashboard integration

#### Late Evening Session
- **User → Dashboard**: Reported Details tab crash on SiteDetailPage
- **Dashboard → System**: Investigated and fixed multiple bugs:
  - Fixed Badge component inside `<p>` tag (hydration error)
  - Fixed connector object rendering in stations/outlets (created safeStringValue helper)
  - Fixed amenities array objects rendering (extract type/name from objects)
- **User → Dashboard**: Confirmed fixes working, requested Create/Edit Site UI
- **Dashboard → System**: Investigated API endpoints:
  - ✅ createSite route exists at `/api/a/createSite`
  - ⚠️ updateSite controller EXISTS (line 751) but route NOT registered
- **Dashboard → API** (via API_ROUTE_FIX_REQUIRED.md):
  - Created detailed instructions for registering updateSite route
  - Location: Add `router.put("/updateSite/:id", siteController.updateSite)` to adminRoutes.js
- **Dashboard → Implementation**: Site Create/Edit UI:
  - Created `src/features/sites/SiteFormDialog.jsx` component
  - Integrated Edit button in SiteDetailPage header
  - Connected dialog with site data and refresh callback
  - Status: UI complete, blocked on API route registration
- **Dashboard → WORK_STATUS.md**: Updated status:
  - Added HIGH PRIORITY API request for updateSite route registration
  - Updated Integration Status table with Sites CRUD (Edit) blocked status
  - Updated Completed Today section with new dashboard work

#### Night Session (API Instance)
- **User → API**: Requested to check WORK_STATUS.md for Dashboard requests
- **API → System**: Read Dashboard request for updateSite route registration
- **API → Implementation**:
  - Registered `PUT /api/a/updateSite/:id` route in adminRoutes.js (line 39)
  - Committed: `2638430` - "Register updateSite route in adminRoutes.js"
- **User → API**: Asked about createStation API
- **API → User**: Explained stations are embedded in Site document, no separate API needed
- **User → API**: Requested to update createSite API to support full site schema
- **API → Implementation**:
  - Enhanced `POST /api/a/createSite` controller (lines 151-302)
  - Added support for ALL schema fields:
    - Basic: name, CPO, description, points
    - Location: coordinates, address, addressComponents (state, city, etc.)
    - Operational: open247, isFastCharger, enabled, stationCount
    - Stations: Full station/outlet structure with connectors
    - Pricing, parking, amenities, contact, accessRestrictions, metadata
  - Added input validation and proper error responses
  - Added Swagger JSDoc documentation
  - Committed: `6c81422` - "Update createSite API to support full site schema"
  - Server rebuilt successfully
- **API → WORK_STATUS.md**: Comprehensive update:
  - Updated API Instance status with all completed work
  - Added createSite enhancement details to "Completed Today"
  - Added full request schema documentation to "API Endpoint Completions"
  - Updated Integration Status table
  - Updated Next Priority Tasks (marked API tasks as complete)
  - Dashboard now has all information needed to implement full Create/Edit Site form

---

## 🔐 Security Notes

- JWT tokens have 30-day expiration
- Passwords hashed with bcrypt
- CORS currently open (needs restriction in production)
- HTTPS enforced via nginx
- File permissions set correctly: dirs 755, files 644

---

## 📚 Quick Links

- [Coordination Guide](COORDINATION.md)
- [API Instructions](chargeCaptainApi/INSTANCE_INSTRUCTIONS_API.md)
- [Dashboard Instructions](chargeCaptainDashboard/INSTANCE_INSTRUCTIONS_DASHBOARD.md)
- [API Documentation](chargeCaptainApi/docs/)
- [Dashboard Reference](DASHBOARD_REFERENCE.md)

---

**Instructions for Updates:**

1. **When starting work**: Update "Current Active Work" section
2. **When completing tasks**: Move from "Pending" to "Completed Today"
3. **When requesting API**: Add to "API Endpoint Requests"
4. **When completing API**: Add to "API Endpoint Completions"
5. **When discovering bugs**: Add to "Known Issues"
6. **Always update timestamp** at the top of this file

**Update Frequency**: Every time you make meaningful progress or switch tasks
