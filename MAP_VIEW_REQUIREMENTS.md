# Map View Requirements - Karmavir's Findings

**Date**: 2024-12-19 21:00
**Documented By**: Dashboard Instance (Claude)
**Findings By**: Karmavir (Product Owner/User)
**Priority**: 🔥 HIGH PRIORITY
**Status**: Requirements gathering complete → Ready for implementation

---

## 📋 User Requirements (Karmavir's Findings)

### 1. Filters in Sidebar ✅

**Requirement**: Map view sidebar should have filter dropdowns

**Filters Needed**:
1. **State Filter** (dropdown)
   - Select state from list of available states
   - Shows count of sites per state
   - When selected: filters map to show only sites in that state

2. **City Filter** (dropdown)
   - Appears after state is selected
   - Shows cities within selected state
   - Shows count of sites per city
   - When selected: filters map to show only sites in that city

3. **Additional Filters** (future enhancement):
   - CPO filter (charging network)
   - Connector type filter
   - 24/7 operation filter
   - Fast charger filter

**UI Placement**:
- Add filter dropdowns ABOVE the search box in sidebar
- Layout:
  ```
  [State Dropdown]
  [City Dropdown] (appears after state selected)
  [Search Box]
  [Sites List]
  ```

### 2. Map Re-Zoom Based on Search ✅

**Requirement**: When user searches in sidebar, map should automatically zoom to show matching results

**Behavior**:
1. User types in search box
2. Sidebar filters sites client-side (name, address, CPO)
3. **Map re-zooms** to fit all matching sites
4. Uses `fitBounds()` to show all filtered markers
5. If only one result: zoom to level 15 and center on that site
6. If multiple results: zoom to show all markers with padding

**Example**:
- User searches "Tesla" → Map zooms to show all Tesla charging stations
- User searches "Houston" → Map zooms to Houston area showing all matches

### 3. Map Re-Zoom Based on Filters ✅

**Requirement**: When user selects state/city filters, map should automatically zoom to show the filtered region

**Behavior**:

**Scenario A - State Selected**:
1. User selects "Texas" from state dropdown
2. API fetches sites filtered by state (server-side)
3. Map re-zooms to fit all Texas sites
4. Uses `fitBounds()` with all Texas site coordinates

**Scenario B - City Selected**:
1. User selects "Texas" → "Houston"
2. API fetches sites filtered by state AND city (server-side)
3. Map re-zooms to fit all Houston sites
4. Zooms to appropriate level for city view (typically level 11-13)

**Scenario C - Filter Cleared**:
1. User clears state/city filter
2. Map resets to default view OR fits bounds of all available sites

### 4. Combined Filter + Search Behavior

**Requirement**: Filters and search should work together

**Example Flow**:
1. User selects State: "California" → Map zooms to California
2. User selects City: "San Francisco" → Map zooms to San Francisco
3. User searches: "Tesla" → Map zooms to show Tesla chargers in San Francisco only
4. User clears search → Map returns to San Francisco view
5. User clears city → Map returns to California view

---

## 🎯 Technical Implementation Requirements

### API Requirements

**1. Enhanced Filtering Endpoint**: `GET /api/a/sites`

**Add Parameters**:
```
?state=Texas              // Filter by state
?city=Houston             // Filter by city
?state=Texas&city=Houston // Combined filter
```

**Response**: Same paginated format as current endpoint

**2. Helper Endpoints**:

**a) Get States**: `GET /api/a/sites/states`
```json
{
  "success": true,
  "data": [
    { "state": "Texas", "count": 245 },
    { "state": "California", "count": 412 },
    { "state": "New York", "count": 189 }
  ],
  "total": 50
}
```

**b) Get Cities**: `GET /api/a/sites/cities?state=Texas`
```json
{
  "success": true,
  "state": "Texas",
  "data": [
    { "city": "Houston", "count": 98 },
    { "city": "Austin", "count": 67 },
    { "city": "Dallas", "count": 54 }
  ],
  "total": 12
}
```

### Dashboard Requirements

**1. Add Filter UI Components** (SitesMapViewPage.jsx)

**Location**: Above search box in sidebar (around line 229)

```jsx
<div className="p-4 border-b space-y-3">
  {/* State Filter */}
  <Select
    value={selectedState}
    onValueChange={handleStateChange}
    placeholder="Select State"
  >
    {states.map(state => (
      <SelectItem value={state.state}>
        {state.state} ({state.count})
      </SelectItem>
    ))}
  </Select>

  {/* City Filter (only shown if state selected) */}
  {selectedState && (
    <Select
      value={selectedCity}
      onValueChange={handleCityChange}
      placeholder="Select City"
    >
      {cities.map(city => (
        <SelectItem value={city.city}>
          {city.city} ({city.count})
        </SelectItem>
      ))}
    </Select>
  )}

  {/* Search Box */}
  <div className="relative">
    <Search className="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4" />
    <Input
      placeholder="Search stations..."
      value={searchQuery}
      onChange={(e) => setSearchQuery(e.target.value)}
      className="pl-9"
    />
  </div>
</div>
```

**2. Add State Management**

```javascript
// New state variables
const [selectedState, setSelectedState] = useState('');
const [selectedCity, setSelectedCity] = useState('');
const [states, setStates] = useState([]);
const [cities, setCities] = useState([]);
```

**3. Fetch States on Mount**

```javascript
useEffect(() => {
  fetchStates();
}, []);

const fetchStates = async () => {
  try {
    const response = await apiClient.get('/sites/states');
    setStates(response.data.data);
  } catch (error) {
    console.error('Error fetching states:', error);
  }
};
```

**4. Fetch Cities When State Changes**

```javascript
const handleStateChange = async (state) => {
  setSelectedState(state);
  setSelectedCity(''); // Clear city selection

  try {
    const response = await apiClient.get(`/sites/cities?state=${state}`);
    setCities(response.data.data);
  } catch (error) {
    console.error('Error fetching cities:', error);
  }

  // Re-fetch sites with state filter
  fetchSites();
};
```

**5. Update fetchSites with Filters**

```javascript
const fetchSites = async () => {
  setLoading(true);
  setError(null);

  try {
    const filters = {};
    if (selectedState) filters.state = selectedState;
    if (selectedCity) filters.city = selectedCity;

    const data = await sitesAPI.getAll(1, 100, filters);
    setSites(data.sites || []);

    // AUTO-ZOOM to filtered results
    if (mapInstanceRef.current && data.sites.length > 0) {
      const bounds = data.sites.map(site => {
        const [lng, lat] = site.location.coordinates;
        return [lat, lng];
      });
      mapInstanceRef.current.fitBounds(bounds, { padding: [50, 50] });
    }
  } catch (err) {
    console.error('Error fetching sites:', err);
    setError(err.response?.data?.message || 'Failed to load sites');
  } finally {
    setLoading(false);
  }
};
```

**6. Update Search Filter Effect**

```javascript
// Update filteredSites to trigger map re-zoom
useEffect(() => {
  if (mapInstanceRef.current && filteredSites.length > 0) {
    const bounds = filteredSites.map(site => {
      const [lng, lat] = site.location.coordinates;
      return [lat, lng];
    });

    if (filteredSites.length === 1) {
      // Single result: center and zoom to level 15
      const [lng, lat] = filteredSites[0].location.coordinates;
      mapInstanceRef.current.setView([lat, lng], 15);
    } else {
      // Multiple results: fit bounds
      mapInstanceRef.current.fitBounds(bounds, { padding: [50, 50] });
    }
  }
}, [filteredSites]);
```

**7. Add Clear Filters Button**

```jsx
{(selectedState || selectedCity) && (
  <Button
    variant="outline"
    size="sm"
    onClick={handleClearFilters}
    className="w-full"
  >
    Clear Filters
  </Button>
)}
```

```javascript
const handleClearFilters = () => {
  setSelectedState('');
  setSelectedCity('');
  setCities([]);
  fetchSites(); // Reload all sites
};
```

---

## 🔄 User Flow with New Features

### Complete Workflow

1. **Initial Load**:
   - User navigates to Map View
   - Map loads first 100 sites (no filters)
   - State dropdown populated with all available states
   - Map fits bounds to show all 100 sites

2. **Filter by State**:
   - User selects "California" from state dropdown
   - API fetches sites filtered by state
   - City dropdown populates with cities in California
   - **Map auto-zooms** to fit all California sites
   - Sidebar shows California sites only

3. **Filter by City**:
   - User selects "San Francisco" from city dropdown
   - API fetches sites filtered by state + city
   - **Map auto-zooms** to San Francisco area
   - Sidebar shows San Francisco sites only

4. **Search Within Filters**:
   - User searches "Tesla" (with CA + SF filters active)
   - **Client-side filter** applied to already-fetched SF sites
   - **Map auto-zooms** to show matching Tesla stations in SF
   - If only 1 result: map centers and zooms to level 15

5. **Clear Search**:
   - User clears search box
   - **Map re-zooms** to fit all San Francisco sites

6. **Clear Filters**:
   - User clicks "Clear Filters"
   - Map reloads unfiltered sites (first 100)
   - **Map re-zooms** to fit all sites

---

## 🎨 UI/UX Design Notes

### Visual Hierarchy
1. **Filters** (top of sidebar) - Primary controls
2. **Search** (below filters) - Secondary refinement
3. **Sites List** (scrollable) - Results display

### Interaction Feedback
- Loading spinner during API calls
- Filter badge showing active filters
- Site count update: "Showing X of Y stations"
- Smooth map transitions (animated zoom)

### Mobile Responsiveness
- Filters stack vertically on mobile
- Map/sidebar split adjusts for smaller screens
- Touch-friendly dropdown and search controls

---

## ✅ Implementation Checklist

### API Instance Tasks
- [ ] Add `state` filter param to `GET /api/a/sites`
- [ ] Add `city` filter param to `GET /api/a/sites`
- [ ] Create `GET /api/a/sites/states` endpoint
- [ ] Create `GET /api/a/sites/cities?state={state}` endpoint
- [ ] Add MongoDB indexes for state/city fields
- [ ] Test filtering with actual data
- [ ] Update Swagger documentation
- [ ] Update `docs/api-quick-reference.md`
- [ ] Notify Dashboard instance via WORK_STATUS.md

### Dashboard Instance Tasks
- [ ] Add state dropdown component
- [ ] Add city dropdown component
- [ ] Fetch states on page load
- [ ] Fetch cities when state changes
- [ ] Update fetchSites to pass state/city filters
- [ ] Add map auto-zoom when filters change
- [ ] Add map auto-zoom when search changes
- [ ] Add "Clear Filters" button
- [ ] Update site count display
- [ ] Test complete user workflow
- [ ] Update WORK_STATUS.md with completion

---

## 📚 Reference Documents

**For API Instance**:
- [Dashboard Map View Analysis (Corrected)](./DASHBOARD_MAP_VIEW_ANALYSIS_CORRECTED.md)
- API Controller: `chargeCaptainApi/server/controllers/siteController.js`
- API Routes: `chargeCaptainApi/server/routes/adminRoutes.js`

**For Dashboard Instance**:
- Map View Component: `src/features/sites/SitesMapViewPage.jsx`
- Sites API Service: `src/api/sites.js`
- shadcn/ui Select Component: Already available

---

## 🚀 Expected Benefits

### Performance
- Reduced initial load (100 sites instead of all sites)
- Faster map rendering (fewer markers)
- Better mobile performance

### User Experience
- Easy geographic navigation (state → city)
- Automatic map zoom (no manual zooming needed)
- Clear visual feedback (filtered results)
- Intuitive filtering workflow

### Scalability
- Works with 10,000+ sites in database
- Maintains performance as dataset grows
- Efficient server-side filtering

---

**Requirements Documented**: 2024-12-19 21:00
**Findings By**: Karmavir
**Status**: ✅ Complete - Ready for implementation by both instances
**Coordination**: Both API and Dashboard instances can work in parallel
