# Charge Captain

**Comprehensive EV Charging Station Feedback Platform**

Charge Captain is a full-stack platform that enables EV users to discover charging stations, provide feedback, and earn rewards. The platform consists of three integrated modules working together to create a complete ecosystem.

## 🏗️ Monorepo Structure

```
chargeCaptain/
├── chargeCaptainApi/          # Backend API (Node.js + Express + MongoDB) [Git Submodule]
├── chargeCaptainDashboard/    # Web Admin Portal (React + Vite) [Git Submodule]
├── cc_mob/                    # Mobile App (Flutter) [Git Submodule]
├── COORDINATION.md            # Multi-instance development guide
├── WORK_STATUS.md             # Real-time work status tracker
└── DASHBOARD_REFERENCE.md     # Dashboard feature reference
```

**Architecture**: This monorepo uses **Git Submodules** to manage three independent repositories:
- Each submodule maintains its own git history and can be developed independently
- The parent repository coordinates all three modules and tracks specific commits
- Submodules can be updated independently without affecting the parent repo

### 🔄 Cloning the Monorepo

**First-time clone** (includes all submodules):
```bash
git clone --recurse-submodules https://github.com/kvjoshi/charge-captain.git
cd charge-captain
```

**If you already cloned without submodules**:
```bash
git clone https://github.com/kvjoshi/charge-captain.git
cd charge-captain
git submodule init
git submodule update
```

### 🔧 Working with Submodules

**Update all submodules to latest commits**:
```bash
git submodule update --remote --merge
```

**Update a specific submodule**:
```bash
cd cc_mob
git pull origin master
cd ..
git add cc_mob
git commit -m "Update cc_mob submodule to latest"
git push
```

**Check submodule status**:
```bash
git submodule status
```

**Submodule Repositories**:
- Mobile App: https://github.com/kvjoshi/cc_mob
- API Server: https://github.com/kvjoshi/chargeCaptainApi
- Dashboard: https://github.com/kvjoshi/chargeCaptainDashboard

## 📦 Modules

### 1. API Server (`chargeCaptainApi/`)

**Status**: ✅ 100% Complete and Production-Ready

**Technology Stack**:
- Node.js + Express 5.0
- MongoDB + Mongoose ODM
- JWT authentication (dual: admin/user)
- AWS S3 for image storage
- ZeptoMail for email delivery
- Swagger/OpenAPI documentation

**Key Features**:
- 23 client user endpoints for mobile app
- 30+ admin endpoints for dashboard
- Geospatial queries with MongoDB 2dsphere indexing
- Points and rewards system with AES-256 encryption
- PlugShare bulk import functionality
- Google Places API enrichment

**Quick Start**:
```bash
cd chargeCaptainApi
npm install
npm start                    # Development with hot reload
```

**Documentation**:
- [API Documentation](chargeCaptainApi/docs/README.md)
- [Controllers Overview](chargeCaptainApi/docs/controllers-overview.md)
- [API Quick Reference](chargeCaptainApi/docs/api-quick-reference.md)
- [Rewards System](chargeCaptainApi/docs/rewards-system.md)

---

### 2. Dashboard (`chargeCaptainDashboard/`)

**Status**: ✅ 100% Complete and Production-Ready

**Technology Stack**:
- React 19 + Vite
- shadcn/ui + Tailwind CSS
- Zustand for state management
- TanStack Table for data grids
- Recharts for analytics
- Leaflet for interactive maps

**Key Features**:
- User management with detailed profiles
- Site management with split-screen map view
- Feedback tracking with image galleries
- Rewards and redemptions management
- System-wide analytics dashboard
- Settings and configuration
- PlugShare import with Google Places enrichment

**Quick Start**:
```bash
cd chargeCaptainDashboard
pnpm install
pnpm dev                     # Runs on http://localhost:5173
```

**Documentation**:
- [Implementation Plan](chargeCaptainDashboard/IMPLEMENTATION_PLAN.md)
- [Dashboard Reference](DASHBOARD_REFERENCE.md)

---

### 3. Mobile Application (`cc_mob/`)

**Status**: 🟡 ~90% Complete (Integration in Progress)

**Technology Stack**:
- Flutter 3.32.6 + Dart 3.8.1
- Provider pattern for state management
- Google Maps integration
- FlutterSecureStorage for secure data
- Dio for API calls

**Key Features**:
- Email-based OTP authentication
- Real-time GPS station discovery
- Multi-card swipeable feedback flow
- Photo capture and upload
- Points and rewards system (UI ready, integration in progress)
- Vehicle management
- Impact tracking

**Quick Start**:
```bash
cd cc_mob
flutter pub get
flutter run                  # Run on connected device/emulator
```

**Documentation**:
- [Mobile App Guide](cc_mob/CLAUDE.md)

---

## 🚀 Getting Started

### Prerequisites

- Node.js 16+ and npm/pnpm
- MongoDB 4.4+
- Flutter 3.x (for mobile development)
- AWS S3 bucket (for image storage)
- Google Maps API key
- ZeptoMail account (for emails)

### Environment Setup

1. **API Server Configuration**:
   ```bash
   cd chargeCaptainApi
   cp .env.example .env
   # Edit .env with your credentials
   ```

   Required variables:
   - `MONGODB_URI` - MongoDB connection string
   - `ADMIN_JWT_SECRET` - Admin JWT secret
   - `C_USER_JWT_SECRET` - Client user JWT secret
   - `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `S3_BUCKET_NAME`
   - `ZEPTOMAIL_TOKEN`
   - `GOOGLE_MAPS_API_KEY`
   - `ENCRYPTION_KEY` - 64-char hex for reward voucher encryption

2. **Start MongoDB**:
   ```bash
   mongod --dbpath /path/to/data
   ```

3. **Start All Services**:
   ```bash
   # Terminal 1 - API Server
   cd chargeCaptainApi && npm start

   # Terminal 2 - Dashboard
   cd chargeCaptainDashboard && pnpm dev

   # Terminal 3 - Mobile App
   cd cc_mob && flutter run
   ```

---

## 🔄 Multi-Instance Development

This monorepo supports parallel development with multiple Claude instances working on different modules simultaneously.

**Coordination System**:
- **WORK_STATUS.md** - Real-time status tracker and communication hub
- **COORDINATION.md** - Complete multi-instance coordination guide

**Instance-Specific Instructions**:
- API Instance: `chargeCaptainApi/INSTANCE_INSTRUCTIONS_API.md`
- Dashboard Instance: `chargeCaptainDashboard/INSTANCE_INSTRUCTIONS_DASHBOARD.md`

**Communication Pattern**:
```markdown
## Instance Name - Status
- **Working On**: Feature/Task description
- **Status**: 🔄 IN PROGRESS / ✅ COMPLETED / ⚠️ BLOCKED
- **Last Update**: 2026-01-18 14:30
- **Files Modified**: List of files
- **Notes**: Important information for other instances
```

---

## 📊 System Architecture

```
┌─────────────────┐
│   Mobile App    │ ──── JWT Auth ───► ┌──────────────┐
│   (Flutter)     │ ◄──── JSON/Images ─┤   API Server │
└─────────────────┘                    │  (Express)   │
                                       └──────┬───────┘
┌─────────────────┐                           │
│   Dashboard     │ ──── JWT Auth ───►        │
│   (React)       │ ◄──── JSON ───────────────┤
└─────────────────┘                           │
                                              │
                                       ┌──────▼───────┐
                                       │   MongoDB    │
                                       │  (Database)  │
                                       └──────────────┘
```

**API Endpoints**:
- `/api/cu/*` - Client user endpoints (mobile app)
- `/api/a/*` - Admin endpoints (dashboard)
- `/docs` - Swagger API documentation

**Authentication**:
- Admin JWT (30-day expiration)
- Client User JWT (30-day expiration)
- Bearer token format

---

## 🎯 Key Features

### For EV Users (Mobile App)
- 📍 Real-time charging station discovery
- 📸 Submit feedback with photos
- ⭐ Earn points for quality feedback
- 🎁 Redeem points for gift cards
- 🚗 Manage multiple vehicles
- 🌱 Track environmental impact

### For Administrators (Dashboard)
- 👥 User management and analytics
- 🗺️ Site management with interactive maps
- 📊 Feedback tracking and CSV export
- 🎁 Rewards and gift card inventory management
- 📈 System-wide analytics and reports
- ⚙️ Configuration and settings
- 📥 PlugShare bulk import with Google Places enrichment

### For CPOs (Dashboard)
- View stations specific to their company
- Access feedback for their locations
- Monitor performance metrics

---

## 🧪 Testing

### API Tests
```bash
cd chargeCaptainApi
npm test                     # Run all tests
npm test:watch               # Watch mode
```

### Mobile Tests
```bash
cd cc_mob
flutter test                 # Run unit tests
flutter analyze              # Static analysis
```

---

## 📚 Documentation

### Project-Wide
- [Multi-Instance Coordination](COORDINATION.md)
- [Work Status Tracker](WORK_STATUS.md)
- [Dashboard Reference](DASHBOARD_REFERENCE.md)

### API Server
- [Complete API Guide](chargeCaptainApi/CLAUDE.md)
- [API Quick Reference](chargeCaptainApi/docs/api-quick-reference.md)
- [Controllers Overview](chargeCaptainApi/docs/controllers-overview.md)
- [Rewards System](chargeCaptainApi/docs/rewards-system.md)
- [Paginated Sites API](chargeCaptainApi/docs/paginated-sites-api.md)

### Dashboard
- [Implementation Plan](chargeCaptainDashboard/IMPLEMENTATION_PLAN.md)
- [Dashboard Guide](chargeCaptainDashboard/CLAUDE.md)

### Mobile App
- [Mobile App Guide](cc_mob/CLAUDE.md)

### API Documentation
- Swagger UI: `http://localhost:3001/docs` (when API server is running)

---

## 🛠️ Development Workflow

### Working on API Features
1. Check `chargeCaptainApi/CLAUDE.md` for guidance
2. Review existing controllers in `docs/controllers-overview.md`
3. Create/update controller in `server/controllers/`
4. Add routes in `server/routes/`
5. Add Swagger JSDoc comments
6. Test with Swagger UI or Postman

### Working on Dashboard Features
1. Check `chargeCaptainDashboard/CLAUDE.md` for guidance
2. Create components in `src/features/`
3. Integrate with API using admin endpoints
4. Test with actual API calls

### Working on Mobile Features
1. Check `cc_mob/CLAUDE.md` for guidance
2. Create screens in `lib/views/`
3. Implement business logic in `lib/controller/`
4. Add state management with Provider
5. Integrate with API using client user endpoints
6. Test on both Android and iOS

---

## 🔒 Security

- Passwords auto-hashed with bcrypt (14 rounds)
- JWT tokens with 30-day expiration
- Helmet for HTTP security headers
- CORS configuration (restrict in production)
- AWS S3 for secure image storage
- AES-256-CBC encryption for gift card vouchers
- Input validation with Zod schemas
- Rate limiting on sensitive endpoints

**Never expose**:
- JWT secrets
- AWS credentials
- Database URIs
- Encryption keys

---

## 📝 License

Proprietary - All rights reserved

---

## 🤝 Contributing

This is a private project. For development coordination:
1. Update `WORK_STATUS.md` with your status
2. Follow instance-specific instructions
3. Communicate via the WORK_STATUS.md file
4. Review `COORDINATION.md` for best practices

---

## 📧 Support

For issues or questions, contact the project administrator.

---

**Built with ❤️ for the EV community**
