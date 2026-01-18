# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Charge Captain** is a comprehensive EV charging feedback platform with three main modules:

1. **API Server** (`chargeCaptainApi/`) - Express.js REST API (100% complete)
2. **Dashboard** (`chargeCaptainDashboard/`) - Web administrative interface (100% complete)
3. **Mobile Application** (`cc_mob/`) - Flutter mobile app for EV users

The platform enables EV users to discover charging stations via mobile app, provide feedback after charging, and earn reward points. Administrators and Charging Point Operators (CPOs) manage stations and view analytics through the dashboard.

### Business Domain

- EV users find stations and submit feedback with photos via mobile app
- Feedback quality determines points earned (via points matrix)
- Admins manage sites, users, and rewards (gift cards)
- CPOs view data specific to their company's stations
- Users redeem points for gift cards entered manually by admins

## Multi-Instance Coordination

**IMPORTANT**: This project supports multiple Claude instances working in parallel on different modules.

### Coordination System

If you are working with multiple Claude instances (one for API, one for Dashboard):

1. **Read COORDINATION.md** - Complete multi-instance coordination guide
2. **Check WORK_STATUS.md** - Real-time work status and communication hub
3. **Follow instance-specific instructions**:
   - API Instance: `chargeCaptainApi/INSTANCE_INSTRUCTIONS_API.md`
   - Dashboard Instance: `chargeCaptainDashboard/INSTANCE_INSTRUCTIONS_DASHBOARD.md`

### File Ownership Rules

- **API Instance**: Only modifies files in `chargeCaptainApi/`
- **Dashboard Instance**: Only modifies files in `chargeCaptainDashboard/`
- **Both**: Update `WORK_STATUS.md` for communication

### Quick Communication Pattern

```markdown
# In WORK_STATUS.md

## Dashboard Instance → API Instance
Need endpoint: POST /api/a/export/sites?format=csv
Purpose: Export sites to CSV
Expected response: CSV file download

## API Instance → Dashboard Instance
Endpoint ready: POST /api/a/export/sites?format=csv
Documentation: docs/api-quick-reference.md line 456
Example: curl -H "Authorization: Bearer token" http://localhost:3001/api/a/export/sites?format=csv
```

**See COORDINATION.md for complete details.**

## Project Structure

```
chargeCaptain/
├── COORDINATION.md            # 🔄 Multi-instance coordination guide
├── WORK_STATUS.md             # 📊 Real-time work status tracker
├── DASHBOARD_REFERENCE.md     # 📖 Complete dashboard feature reference
│
├── chargeCaptainApi/          # Backend API (Express + MongoDB)
│   ├── server/                # Source code
│   ├── dist/                  # Transpiled production code
│   ├── docs/                  # Comprehensive API documentation
│   ├── CLAUDE.md              # API-specific guidance
│   └── INSTANCE_INSTRUCTIONS_API.md  # 🤖 Instructions for API instance
│
├── chargeCaptainDashboard/    # Web admin portal (Vite + React)
│   ├── src/                   # Source code
│   │   ├── api/               # API client and services
│   │   ├── components/        # Reusable components
│   │   ├── features/          # Feature modules (pages)
│   │   ├── stores/            # Zustand state management
│   │   └── ...
│   ├── IMPLEMENTATION_PLAN.md # Detailed implementation guide
│   ├── CLAUDE.md              # Dashboard-specific guidance
│   └── INSTANCE_INSTRUCTIONS_DASHBOARD.md  # 🤖 Instructions for Dashboard instance
│
├── cc_mob/                    # Mobile app (Flutter + Dart)
│   ├── lib/                   # Source code
│   │   ├── views/             # UI screens
│   │   ├── controller/        # Business logic
│   │   ├── providers/         # State management
│   │   ├── models/            # Data models
│   │   └── ...
│   ├── CLAUDE.md              # Mobile-specific guidance
│   └── pubspec.yaml           # Flutter dependencies
│
└── CLAUDE.md                  # This file - project-wide guidance
```

## Working with the API Server

### Location
All API server code is in `chargeCaptainApi/` directory.

### Documentation
**IMPORTANT**: Before working on API features, consult:
- `chargeCaptainApi/CLAUDE.md` - Comprehensive API development guide
- `chargeCaptainApi/docs/README.md` - API architecture and workflows
- `chargeCaptainApi/docs/controllers-overview.md` - Complete controller reference
- `chargeCaptainApi/docs/api-quick-reference.md` - Endpoint lookup table

### Key Commands (run from `chargeCaptainApi/`)

```bash
# Development with hot reload
npm start                    # Uses nodemon, auto-rebuilds on changes

# Build and run
npm run build               # Transpile server/ → dist/ with Babel
npm run dev                 # Build + start server
npm run prod                # Production build + start

# Testing and quality
npm test                    # Run Jest tests
npm test:watch              # Run tests in watch mode
npm run lint                # ESLint
npm run docs                # Generate JSDoc documentation
```

### API Architecture

**Technology Stack**:
- Node.js + Express 5.0
- MongoDB + Mongoose ODM
- JWT authentication (separate secrets for admin/users)
- AWS S3 for image storage
- Babel transpilation (ES6+ → CommonJS)
- Swagger/OpenAPI docs at `/docs`

**Route Structure**:
- `/api/a/*` - Admin routes (protected with `adminProtect` middleware)
- `/api/cu/*` - Client user routes (protected with `clientUserProtect` middleware)

**Core Models**:
- `Site` - Charging stations with geospatial indexing (2dsphere)
- `Feedback` - User feedback with points calculation
- `clientUser` - EV users with points system and vehicle management
- `Admin` - Administrative users
- `RewardProduct` - Reward categories (e.g., Amazon Pay Gift Card)
- `RewardVariant` - Product denominations (₹100, ₹250, ₹500)
- `GiftCard` - Individual vouchers with AES-256 encryption
- `Redemption` - Points redemption transaction logs

### Critical API Concepts

1. **Dual Authentication System**:
   - Admin JWT: `ADMIN_JWT_SECRET` (30-day expiration)
   - Client User JWT: `C_USER_JWT_SECRET` (30-day expiration)
   - Both use Bearer token format

2. **Geospatial Queries**:
   - Coordinates format: `[longitude, latitude]` (GeoJSON standard)
   - Uses MongoDB 2dsphere index on `sites.location`
   - Example: `getNearestSites` uses `$near` operator

3. **Points System**:
   - Calculated in `feedbackController.js`
   - Points added to both `lifetimePoints` and `totalAvailablePoints`
   - `pointsHistory[]` tracks all transactions for audit

4. **PlugShare Integration**:
   - Single import: `/api/a/single-plugshare`
   - Bulk ZIP import: `/api/a/upload-plugshare`
   - Helper: `decodePlugshare.js` parses PlugShare format

5. **Image Handling**:
   - Base64 or FormData upload
   - Route with multer: `/api/cu/siteFeedbackFD`
   - Stored in AWS S3 with presigned URLs

6. **Rewards System**:
   - Manual gift card management by admins
   - Product → Variant hierarchy (Amazon Pay → ₹100, ₹250, ₹500)
   - AES-256-CBC encryption for voucher codes
   - FIFO redemption (nearest expiry distributed first)
   - Email delivery of voucher codes via ZeptoMail
   - Points deducted from `totalAvailablePoints` on redemption
   - Transaction safety via MongoDB transactions
   - Analytics: inventory, redemptions, low stock alerts, expiry alerts
   - Documentation: `docs/rewards-system.md`

7. **Paginated Sites API**:
   - Mobile API: `GET /api/cu/sites` - Lightweight, optimized (~70% smaller)
   - Admin API: `GET /api/a/sites` - Full data with advanced filtering
   - Geospatial search with distance calculation (Haversine formula)
   - Server-side pagination, filtering, sorting
   - Admin stats: totalStations, availableStations, fastChargers
   - Documentation: `docs/paginated-sites-api.md`

## Working with the Dashboard

### Status
The dashboard module is **100% complete** and production-ready! 🎉

**Location**: `chargeCaptainDashboard/` directory

**Technology Stack**: Vite + React 19 (JavaScript), shadcn/ui, Tailwind CSS, Zustand, React Router v6

**Documentation**: See `chargeCaptainDashboard/CLAUDE.md` for complete dashboard-specific guidance

### Completed Modules (All 9 Phases)

**✅ Authentication & Layout**:
- Login page with redesigned UI (centered card, light gradient)
- JWT-based authentication with Zustand
- Protected routes with AuthGuard
- Responsive sidebar and header

**✅ User Management**:
- Users list with search, filters, pagination
- User detail page with tabs (info, vehicles, points, feedback, redemptions)
- Points history tracking
- Activate/deactivate users

**✅ Site Management**:
- Sites list with comprehensive filters
  - **State/city filtering** with cascading dropdowns (state → city)
  - Place ID filter for sites with Google Places integration
  - Search, connector type, 24/7, fast charger filters
  - Integrated "Create Site" button
- Site detail page with map integration
  - Edit site functionality with full schema support
  - Detailed tabs view (Overview, Details, Feedback)
- **Comprehensive Site Create page** with tabbed interface:
  - Basic Info tab: Name, CPO, description, points
  - Location tab: Interactive map for coordinate selection, address fields
  - Stations tab: Dynamic station/outlet management with connector types
  - Details tab: Pricing, parking, amenities, contact info
  - Full schema support matching backend Site model
- **Split-screen map view** with all charging stations displayed on interactive map with synchronized sidebar
  - **State/city filtering** with auto-zoom to filtered results
  - Search with auto-zoom to matching sites
  - Custom markers with popup details
  - Bidirectional selection (map ↔ sidebar)
- Google Maps click-to-open functionality (uses Place ID when available)
- PlugShare import (single & bulk ZIP)
- Google Places enrichment
- **Optimized Sites API**: Using new paginated `GET /api/a/sites` endpoint with advanced filtering and statistics
- **Data transformation layer** to bridge nested backend structure with flat frontend expectations

**✅ Feedback Management**:
- Feedback list with statistics dashboard
- Searchable data table
- CSV export functionality
- Detail modal with image gallery (lightbox with keyboard navigation)
- Navigation links to users and sites

**✅ Rewards System**:
- Products and variants management
- Gift cards inventory
- Redemptions tracking
- Analytics dashboard (inventory, redemptions, alerts)

**✅ Analytics & Reports**:
- System-wide analytics dashboard
- Growth trends (users, feedback over time)
- Distribution charts (CPO breakdown, top stations, connector types)
- Points economy tracking (issued, redeemed, available)
- Interactive Recharts visualizations

**✅ Settings**:
- Admin user management (add, edit, delete)
- Points system configuration (default points, photo bonus, minimum redemption)
- API configuration (Google Maps, ZeptoMail)
- General settings (site title, support email, pagination defaults, timezone)

### Key Architectural Decisions

1. **Data Transformation Layer**: Implemented in `src/api/sites.js` to transform nested MongoDB objects (location.address, CPO) to flat frontend structure (address, cpoName)

2. **Optimized Sites API**: Updated to use new paginated `GET /api/a/sites` endpoint with server-side pagination, filtering, sorting, and aggregated statistics

3. **State/City Filtering System**:
   - Backend: MongoDB indexes on `location.addressComponents.administrative_area_1` and `locality`
   - API endpoints: `GET /api/a/sites/states` and `GET /api/a/sites/cities?state=X`
   - Frontend: Cascading dropdowns in List and Map views with auto-zoom functionality

4. **Comprehensive Site Create Page**: Tabbed interface (`src/features/sites/SiteCreatePage.jsx`) supporting full site schema with interactive map, dynamic station management, and live preview

5. **Split-Screen Map View**: Interactive Leaflet map (`src/features/sites/SitesMapViewPage.jsx`) with synchronized sidebar, custom markers, bidirectional selection, and auto-zoom on filter/search

6. **Google Maps Integration**: Smart URL generation using Place ID when available, falling back to coordinates

7. **Image Gallery Component**: Reusable lightbox component (`src/components/common/ImageGallery.jsx`) with keyboard navigation for feedback photos

8. **Analytics Dashboard**: Comprehensive system analytics with Recharts visualizations for growth trends, distribution analysis, and points economy tracking

9. **Safe localStorage Handling**: All stores use `safeJSONParse` helper to prevent crashes from corrupted JSON data

### Integration Points
Dashboard consumes API via:
- Admin endpoints: `/api/a/*`
- Authentication: Admin JWT tokens stored in localStorage
- Axios interceptors for automatic token attachment and 401 handling
- Data transformation layer for nested-to-flat structure conversion
- Swagger docs for reference: `http://localhost:3001/docs`

### Development Commands

```bash
# Start dashboard (from chargeCaptainDashboard/)
pnpm dev                      # Runs on http://localhost:5173

# Requires API server running separately
cd ../chargeCaptainApi && npm start
```

## Mobile Application

### Status
The mobile module is a Flutter application for EV users.

**Location**: `cc_mob/` directory

**Technology Stack**: Flutter 3.32.6 + Dart 3.8.1, Provider pattern for state management, Dio for API calls

**Documentation**: See `cc_mob/CLAUDE.md` for complete mobile-specific guidance

### Key Features

**✅ Authentication & Onboarding**:
- Email-based authentication with OTP verification
- User registration and profile setup
- Secure storage for user data and tokens

**✅ Station Discovery**:
- Location-based charging station search
- Google Maps integration for station display
- Real-time distance calculation
- Station details with connector information

**✅ Feedback System**:
- Multi-card swipeable feedback flow
- Photo capture and upload (camera/gallery)
- Points awarded for quality feedback
- Metadata collection (charger details, experience rating)

**✅ User Profile & Vehicles**:
- Vehicle management (add/delete)
- Profile information management
- Points and rewards tracking

**✅ Impact Tracking**:
- Environmental impact visualization
- User engagement metrics

### Integration Points
Mobile app consumes:
- Client user endpoints: `/api/cu/*`
- Authentication: Client user JWT tokens (stored in FlutterSecureStorage)
- Base API: `https://api.chargecaptain.com/api`
- Key endpoints:
  - `POST /api/cu/verify` - User verification
  - `POST /api/cu/register` - Registration with OTP
  - `POST /api/cu/validateOTP` - OTP validation
  - `GET /api/cu/sites` - Paginated sites (optimized for mobile)
  - `POST /api/cu/siteFeedbackFD` - Feedback submission with images
  - `POST /api/cu/addVehicle` - Vehicle management
  - `GET /api/cu/rewards/catalog` - Rewards catalog
  - `POST /api/cu/rewards/claim` - Redeem points

### Development Commands

```bash
# Run app (from cc_mob/)
flutter run                    # Run on connected device/emulator
flutter run --release          # Run release build

# Build
flutter build apk              # Build Android APK
flutter build appbundle        # Build Android App Bundle (for Play Store)

# Testing and quality
flutter test                   # Run tests
flutter analyze                # Static analysis
flutter clean                  # Clean build artifacts
```

## Environment Configuration

### Required Variables
Located in `chargeCaptainApi/.env`:

```env
# Server
PORT=3001

# Database
MONGODB_URI=mongodb://localhost:27017/chargecaptain

# Authentication
ADMIN_JWT_SECRET=your_admin_secret
C_USER_JWT_SECRET=your_user_secret

# AWS S3
AWS_ACCESS_KEY_ID=your_key
AWS_SECRET_ACCESS_KEY=your_secret
AWS_REGION=us-east-1
S3_BUCKET_NAME=your_bucket

# Email (ZeptoMail)
ZEPTOMAIL_TOKEN=your_token

# Google Maps
GOOGLE_MAPS_API_KEY=your_api_key

# Rewards System (AES-256 Encryption)
ENCRYPTION_KEY=your_64_char_hex_encryption_key
```

**Generate Encryption Key**:
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```
See `REWARDS_SETUP.md` for detailed setup instructions.

## Development Workflow

### Starting a New Task

1. **API Work**:
   - Read `chargeCaptainApi/CLAUDE.md` for detailed guidance
   - Check existing controllers in `chargeCaptainApi/docs/controllers-overview.md`
   - Navigate to `chargeCaptainApi/` directory
   - Run `npm start` for development

2. **Dashboard Work**:
   - Read `chargeCaptainDashboard/CLAUDE.md` for detailed guidance
   - Review API documentation for integration requirements
   - Navigate to `chargeCaptainDashboard/` directory
   - Run `pnpm dev` for development

3. **Mobile Work**:
   - Read `cc_mob/CLAUDE.md` for detailed guidance
   - Review API documentation for client user endpoints (`/api/cu/*`)
   - Navigate to `cc_mob/` directory
   - Run `flutter run` for development

4. **Cross-Module Features**:
   - API changes: Update controllers, test with Swagger
   - Dashboard integration: Verify with admin API endpoints (`/api/a/*`)
   - Mobile integration: Verify with client user endpoints (`/api/cu/*`)
   - Document API changes in relevant module documentation

### Adding New Features

**API Features**:
1. Create/update controller in `server/controllers/`
2. Add routes in `server/routes/` (adminRoutes.js or clientUserRoutes.js)
3. Add Swagger JSDoc comments for documentation
4. Update models if needed in `server/models/`
5. Test with Postman or Swagger UI
6. Run `npm run lint` and `npm test`

**Dashboard Features**:
1. Create React components in `src/features/`
2. Integrate with API using admin JWT (`/api/a/*` endpoints)
3. Handle authentication state with Zustand
4. Implement responsive design with Tailwind CSS
5. Test with actual API calls

**Mobile Features**:
1. Create Flutter screens in `lib/views/`
2. Implement business logic in `lib/controller/`
3. Add state management with Provider in `lib/providers/`
4. Integrate with API using client user JWT (`/api/cu/*` endpoints)
5. Handle image uploads and location permissions
6. Test on both Android and iOS devices

## Security Considerations

- **Passwords**: Auto-hashed via bcrypt in Mongoose pre-save hooks
- **JWT Tokens**: Stateless authentication, 30-day expiration
- **CORS**: Currently open (`origin: "*"`) - **must restrict in production**
- **Request Limits**: 50mb for JSON/URL-encoded (for image uploads)
- **Helmet**: HTTP security headers enabled
- **Input Validation**: Use Zod schemas where implemented
- **Never expose**: JWT secrets, AWS credentials, database URIs

## Common Development Tasks

### API Server

```bash
# Start development server
cd chargeCaptainApi && npm start

# Run specific test file
npm test -- path/to/test.spec.js

# Check logs
tail -f chargeCaptainApi/logs/app.log

# Access Swagger docs
# Navigate to http://localhost:3001/docs
```

### Database Operations

```bash
# Connect to MongoDB
mongosh mongodb://localhost:27017/chargecaptain

# Common queries
db.sites.find().limit(5)
db.clientusers.countDocuments()
db.feedbacks.find({userId: "..."})
```

### Troubleshooting

**API Server Won't Start**:
- Check MongoDB is running: `mongosh`
- Verify `.env` exists in `chargeCaptainApi/`
- Check port 3001 is available
- Review logs in `chargeCaptainApi/logs/`

**OTP Not Sending**:
- Email sending is currently commented out (clientUserController.js:95)
- OTP logged to console in development
- Check ZeptoMail configuration for production

**Geospatial Queries Failing**:
- Ensure 2dsphere index exists: `db.sites.getIndexes()`
- Verify coordinates are `[longitude, latitude]` order
- Check if site locations are properly populated

**Image Upload Fails**:
- Max size: 5MB per image
- Max images: 10 per feedback
- Field names must be: `image_0`, `image_1`, etc.
- Check AWS S3 credentials and permissions

## Code Style Guidelines

### API Server
- Use ES6+ syntax (imports, arrow functions, async/await)
- Wrap async routes with `express-async-handler`
- Add Swagger JSDoc comments for all endpoints
- Use Mongoose for all database operations
- Error handling: Leverage Express 5.0 error handling

### Dashboard
- Follows React 19 best practices
- JavaScript (TypeScript migration planned for future)
- Component-based architecture with feature modules
- State management: Zustand for global state, React Hook Form for forms
- API calls via centralized Axios client with interceptors
- shadcn/ui + Tailwind CSS for UI components
- TanStack Table for data tables, Recharts for charts
- React Router v6 for routing

## Testing Strategy

### API Tests
- Located in `tests/` directory
- Use Jest + Supertest for endpoint testing
- Test authentication flows
- Test geospatial queries
- Mock AWS S3 and email services

### Dashboard Tests (future)
- Component tests with React Testing Library
- E2E tests with Playwright or Cypress
- API integration tests

## Package Management

The API uses **pnpm** as package manager (see package.json).

```bash
# Install dependencies
pnpm install

# Add new package
pnpm add <package-name>

# Add dev dependency
pnpm add -D <package-name>
```

## API Endpoints Summary

Full reference available in `chargeCaptainApi/docs/api-quick-reference.md`.

**Key Admin Endpoints**:
- `POST /api/a/login` - Admin authentication
- `GET /api/a/getAllUsers` - List all client users
- `GET /api/a/sites` - **NEW** Paginated sites with advanced filtering
- `POST /api/a/addSite` - Create charging station
- `POST /api/a/upload-plugshare` - Bulk PlugShare import
- `POST /api/a/enrichSitesWithPlaceIds` - Google Places enrichment
- `POST /api/a/rewards/products` - Create reward product
- `POST /api/a/rewards/giftcards` - Add gift card vouchers
- `GET /api/a/rewards/analytics/inventory` - Inventory analytics
- `GET /api/a/rewards/redemptions` - Redemption history

**Key Client User Endpoints**:
- `POST /api/cu/verify` - Check if user exists
- `POST /api/cu/register` - User registration
- `POST /api/cu/validateOTP` - OTP verification
- `GET /api/cu/sites` - **NEW** Paginated sites (optimized for mobile)
- `POST /api/cu/siteFeedbackFD` - Submit feedback with images
- `GET /api/cu/rewards/catalog` - Browse rewards catalog
- `POST /api/cu/rewards/claim` - Redeem points for reward
- `GET /api/cu/rewards/my-redemptions` - User's redemption history

## Additional Resources

### API Documentation
- Swagger API Docs: `http://localhost:3001/docs`
- Complete API Guide: `chargeCaptainApi/CLAUDE.md`
- API Quick Reference: `chargeCaptainApi/docs/api-quick-reference.md`
- Controllers Overview: `chargeCaptainApi/docs/controllers-overview.md`

### Feature-Specific Documentation
- **Rewards System**: `docs/rewards-system.md` - Complete rewards implementation guide
- **Rewards Setup**: `REWARDS_SETUP.md` - Encryption key generation and configuration
- **Manual Redemption Mode**: `docs/manual-redemption-mode-plan.md` - Future enhancement plan
- **Paginated Sites API**: `docs/paginated-sites-api.md` - Complete API reference
- **Mobile Integration**: `docs/mobile-app-sites-integration.md` - React Native examples
- **Dashboard Integration**: `docs/dashboard-sites-integration.md` - Web dashboard examples

### External Documentation
- Mongoose Docs: https://mongoosejs.com/
- Express 5.0 Docs: https://expressjs.com/en/5x/api.html
- MongoDB Geospatial Queries: https://www.mongodb.com/docs/manual/geospatial-queries/

## Module-Specific Guidance

When working within a specific module:
- **API**: Always refer to `chargeCaptainApi/CLAUDE.md` for detailed API guidance
- **Dashboard**: Always refer to `chargeCaptainDashboard/CLAUDE.md` for detailed dashboard guidance
- **Mobile**: Always refer to `cc_mob/CLAUDE.md` for detailed mobile app guidance