

# FitMart

### *Your All-in-One Fitness & Nutrition E-Commerce Platform*

> A full-stack MERN e-commerce application combining premium fitness gear, nutrition products, workout tracking, and seamless payments — built for learning, collaboration, and real-world use.


---

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Tech Stack](#-tech-stack)
- [High-Level Architecture](#-high-level-architecture)
- [Project Structure](#-project-structure)
- [File-to-File Handoff Chain](#-file-to-file-handoff-chain)
  - [Client Startup Chain](#client-startup-chain)
  - [Server Startup Chain](#server-startup-chain)
  - [Docker Startup Chain](#docker-startup-chain)
- [How to Run the Project](#-how-to-run-the-project)
  - [Prerequisites](#prerequisites)
  - [Environment Variables](#-environment-variables)
  - [Seed the Database](#-seed-the-database)
  - [Run in Development](#-run-in-development)
  - [Run with Docker](#-run-with-docker-optional)
- [User Journey Through the Project](#-user-journey-through-the-project)
  - [Journey 1 — New Visitor → First Purchase](#journey-1--new-visitor--first-purchase)
  - [Journey 2 — Workout & Fitness Tracking](#journey-2--workout--fitness-tracking)
  - [Journey 3 — Admin Panel](#journey-3--admin-panel)
- [Impact & What This Project Demonstrates](#-impact--what-this-project-demonstrates)


---

## 🧠 Project Overview

**FitMart** is a production-grade, full-stack **MERN** (MongoDB, Express, React, Node.js) e-commerce platform for fitness equipment, nutrition products, and wearables. It goes far beyond a simple storefront by integrating fitness tracking tools, AI-powered assistance, and a complete admin management system.

### Core Capabilities

| Area | Features |
|------|----------|
| **E-Commerce** | Product catalog with search & filters, smart cart with real-time stock reservation, Razorpay payments with HMAC verification, order management with price snapshotting, welcome discount for first-time buyers |
| **Fitness & Wellness** | BMI/TDEE/Calorie calculators with product recommendations, workout tracker (FullCalendar-based fitness calendar), workout notes with exercise logging & GIF previews (ExerciseDB API), nearby fitness center discovery, curated fitness plans (weight loss, muscle building, mobility & recovery) |
| **AI Integration** | Fitness chatbot powered by Google Gemini 2.5 Flash for workout and nutrition queries |
| **Admin Panel** | Dashboard with revenue KPIs & charts, real-time inventory management, customer segmentation (new / returning / high-value), sales reports (daily/weekly/monthly), marketing strategies, bug tracker |
| **Engagement** | Automated transactional emails (first-purchase welcome, inactivity re-engagement), FitRewards loyalty program, in-app bug reporting |
| **Security** | Firebase authentication (Email + Google Sign-In), rate limiting, Helmet security headers, Zod input validation, resource ownership guards |

---

## 🛠️ Tech Stack

### Frontend

| Technology | Version | Purpose |
|-----------|---------|---------|
| **React** | v19 | Component-based UI framework |
| **Vite** | v7 | Lightning-fast build tool with HMR |
| **Tailwind CSS** | v4 | Utility-first CSS framework |
| **React Router** | v7 | Client-side SPA routing |
| **Firebase Client SDK** | v12 | User authentication (Email + Google) |
| **Framer Motion** | v12 | Page transitions & micro-animations |
| **FullCalendar** | v6 | Interactive workout calendar widget |
| **Recharts** | v3 | Admin dashboard charts (Area, Bar) |
| **TanStack React Query** | v5 | Server state management & infinite scroll |
| **DOMPurify + Marked** | latest | Safe markdown rendering for chatbot |

### Backend

| Technology | Version | Purpose |
|-----------|---------|---------|
| **Node.js** | v16+ | JavaScript runtime |
| **Express** | v4 | HTTP server & routing framework |
| **Mongoose** | v7 | MongoDB ODM with schema validation |
| **Firebase Admin SDK** | v13 | Server-side JWT token verification |
| **Razorpay SDK** | v2 | Payment order creation & HMAC verification |
| **Nodemailer** | v8 | SMTP email transporter |
| **Helmet** | v8 | HTTP security headers |
| **express-rate-limit** | v8 | API & payment rate limiting |
| **@google/generative-ai** | v0.24 | Gemini 2.5 Flash AI chatbot |
| **Zod** | v4 | Request body validation schemas |
| **Redis** | v6 | Optional product list caching |

### Database & Infrastructure

| Service | Role |
|---------|------|
| **MongoDB** (Atlas or Docker) | Primary data store for products, orders, carts, users, bugs |
| **Redis** (optional) | Caches paginated product queries (TTL-based) |
| **Firebase** | Authentication provider |
| **Razorpay** | Indian payment gateway (UPI, cards, wallets) |
| **ExerciseDB (RapidAPI)** | Exercise library with animated GIF demonstrations |
| **SMTP (Gmail)** | Transactional welcome & re-engagement emails |
| **Docker + Docker Compose** | Containerized full-stack orchestration |
| **Vercel** | Frontend deployment (pre-configured `vercel.json`) |

### Dev & Testing

| Tool | Purpose |
|------|---------|
| **Jest** | Server-side unit testing |
| **Supertest** | HTTP assertion library for Express routes |
| **Nodemon** | Auto-restart server on file changes |
| **ESLint** | Code linting for client |

---

## 🏗️ High-Level Architecture

```mermaid
graph TB
    subgraph "CLIENT (React + Vite)"
        A["index.html"] --> B["main.jsx"]
        B --> C["App.jsx (Router)"]
        C --> D["Pages"]
        C --> E["Components"]
        D --> F["Auth / Firebase"]
        E --> G["API Calls via fetch()"]
    end

    subgraph "SERVER (Node.js + Express)"
        H["index.js (Entry)"] --> I["Middleware Stack"]
        I --> J["API Routes (/api/*)"]
        J --> K["Models (Mongoose)"]
        J --> L["Services"]
        H --> M["db.js (MongoDB)"]
        H --> N["firebaseAdmin.js"]
    end

    subgraph "EXTERNAL SERVICES"
        O["Firebase Auth"]
        P["MongoDB Atlas"]
        Q["Razorpay"]
        R["Google Gemini AI"]
        S["ExerciseDB (RapidAPI)"]
        T["SMTP (Nodemailer)"]
        U["Redis Cache"]
    end

    G -- "REST API (HTTP)" --> J
    F -- "Auth Tokens" --> O
    N -- "Token Verification" --> O
    M -- "Mongoose ODM" --> P
    J -- "Payment SDK" --> Q
    J -- "Generative AI" --> R
    J -- "Exercise Data" --> S
    L -- "Email Delivery" --> T
    J -- "Product Caching" --> U

    style A fill:#1a1a2e,color:#fff
    style H fill:#16213e,color:#fff
    style O fill:#ff9800,color:#000
    style P fill:#47A248,color:#fff
    style Q fill:#0C2A5E,color:#fff
    style R fill:#4285F4,color:#fff
```

| Layer | Responsibility | Key Files |
|-------|---------------|-----------|
| **Browser** | Renders React SPA, manages client state, sends API requests | `index.html`, `main.jsx`, `App.jsx` |
| **Vite Dev Server** | Hot Module Replacement, asset bundling, env injection | `vite.config.js`, `.env` |
| **Express API** | REST endpoints, auth middleware, business logic | `server/index.js`, `routes/*.js` |
| **Mongoose ODM** | Schema definitions, DB queries, validations | `models/*.js`, `db.js` |
| **Services Layer** | Email, order creation, AI chat config | `services/*.js`, `config/*.js` |
| **External APIs** | Auth, payments, AI, exercise data, email | Firebase, Razorpay, Gemini, ExerciseDB, SMTP |

---

## 📁 Project Structure

```
FitMart/
│
├── client/                          # ── REACT FRONTEND ──────────────
│   ├── public/                      # Static assets (logo, icons)
│   ├── src/
│   │   ├── auth/                    # Authentication layer
│   │   │   ├── firebase.js          #   Firebase app init + auth export
│   │   │   ├── useAuth.js           #   React hook: onAuthStateChanged
│   │   │   └── useWelcomeDiscount.js #  First-time discount banner logic
│   │   │
│   │   ├── components/              # Reusable UI components
│   │   │   ├── Navbar.jsx           #   Dual-variant nav (landing/home)
│   │   │   ├── CartDrawer.jsx       #   Slide-in cart panel
│   │   │   ├── FitnessChatBot.jsx   #   Floating AI chat (Gemini)
│   │   │   ├── BMICalculator.jsx    #   BMI/TDEE calculator
│   │   │   ├── CalorieCalculator.jsx #  Daily calorie targets
│   │   │   ├── WorkoutCalendar.jsx  #   FullCalendar widget
│   │   │   ├── NearbyFitnessCenters.jsx # Gym/studio discovery
│   │   │   ├── AdminRoute.jsx       #   Admin route guard
│   │   │   ├── NonAdminRoute.jsx    #   Non-admin route guard
│   │   │   ├── ReportBugButton.jsx  #   In-app bug reporter
│   │   │   ├── WelcomeBanner.jsx    #   First-visit discount banner
│   │   │   └── ...                  #   Additional components
│   │   │
│   │   ├── pages/                   # Route-level page components
│   │   │   ├── LandingPage.jsx      #   Marketing homepage (/)
│   │   │   ├── Authentication.jsx   #   Sign in/up (/auth)
│   │   │   ├── HomePage.jsx         #   Product catalog (/home)
│   │   │   ├── ProductPage.jsx      #   Product detail (/product/:id)
│   │   │   ├── Checkout.jsx         #   Order review (/checkout)
│   │   │   ├── PaymentPage.jsx      #   Payment flow (/payment)
│   │   │   ├── ProductConfirmation.jsx # Success screen
│   │   │   ├── Profile.jsx          #   User profile (/profile)
│   │   │   ├── TrackerPage.jsx      #   Workout calendar (/tracker)
│   │   │   ├── NotesPage.jsx        #   Workout notes (/notes)
│   │   │   ├── ExercisePage.jsx     #   Exercise browser (/exercises)
│   │   │   ├── AdminDashboard.jsx   #   Admin KPIs (/admin/dashboard)
│   │   │   ├── AdminInventory.jsx   #   Stock mgmt (/admin/inventory)
│   │   │   ├── AdminCustomers.jsx   #   Customer list (/admin/customers)
│   │   │   ├── AdminReports.jsx     #   Sales reports (/admin/reports)
│   │   │   └── ...                  #   Additional pages
│   │   │
│   │   ├── hooks/
│   │   │   └── useInfiniteProducts.js # React Query infinite scroll hook
│   │   │
│   │   ├── utils/                   # Utility functions
│   │   │   ├── getAuthHeaders.js    #   Firebase token → Auth header
│   │   │   ├── healthUtils.js       #   BMI, BMR, TDEE math
│   │   │   ├── workoutStorage.js    #   localStorage workout CRUD
│   │   │   ├── formatters.js        #   INR currency formatter
│   │   │   ├── normalizeProduct.js  #   Normalize product ID fields
│   │   │   └── rewardsUtils.js      #   FitRewards helper functions
│   │   │
│   │   ├── App.jsx                  # Root: Router + QueryClient + ErrorBoundary
│   │   ├── main.jsx                 # React DOM mount point
│   │   └── index.css                # Tailwind CSS import
│   │
│   ├── index.html                   # HTML shell
│   ├── vite.config.js               # Vite configuration
│   ├── vercel.json                  # Vercel SPA rewrite rules
│   ├── Dockerfile                   # Multi-stage build (Vite → Nginx)
│   └── package.json                 # Dependencies & scripts
│
├── server/                          # ── NODE.JS BACKEND ─────────────
│   ├── config/
│   │   ├── constants.js             #   Rate limits, thresholds, port
│   │   ├── chatConfig.js            #   Gemini system prompt & config
│   │   └── rewardsConfig.js         #   FitRewards points per rupee
│   │
│   ├── middleware/
│   │   ├── verifyFirebaseToken.js   #   JWT verification middleware
│   │   ├── verifyAdmin.js           #   Admin UID authorization
│   │   ├── logger.js                #   Colored HTTP request logger
│   │   ├── ownership.js             #   Resource ownership check
│   │   └── validateRequest.js       #   Zod schema validation
│   │
│   ├── models/                      # Mongoose schemas
│   │   ├── Product.js               #   Product catalog schema
│   │   ├── Cart.js                  #   Shopping cart schema
│   │   ├── Order.js                 #   Order with price snapshots
│   │   ├── UserProfile.js           #   Extended user profile
│   │   ├── Bug.js                   #   Bug report schema
│   │   ├── FitnessCenter.js         #   Gym/studio schema
│   │   ├── Rewards.js               #   Loyalty points schema
│   │   └── WorkoutLog.js            #   Server-side workout log
│   │
│   ├── routes/                      # Express route handlers
│   │   ├── products.js              #   CRUD + pagination + caching
│   │   ├── cart.js                  #   Cart + stock reservation
│   │   ├── orders.js                #   Order creation & listing
│   │   ├── payment.js               #   Razorpay create/verify/demo
│   │   ├── user.js                  #   Profile, discount, addresses
│   │   ├── chat.js                  #   Gemini AI chatbot
│   │   ├── exercises.js             #   ExerciseDB API proxy
│   │   ├── bugs.js                  #   Bug CRUD + admin status
│   │   ├── dashboard.js             #   Admin KPIs & charts data
│   │   ├── customers.js             #   Customer directory + detail
│   │   ├── reports.js               #   Sales analytics
│   │   ├── fitnessCenters.js        #   Nearby gym discovery
│   │   ├── rewards.js               #   FitRewards points API
│   │   └── workouts.js              #   Workout log endpoints
│   │
│   ├── services/                    # Business logic services
│   │   ├── orderService.js          #   Order creation + stock deduction
│   │   ├── emailService.js          #   Nodemailer SMTP transporter
│   │   ├── emailTemplates.js        #   HTML/text email templates
│   │   ├── firstPurchaseEmailService.js # Welcome email logic
│   │   └── inactiveCustomerEmailService.js # Re-engagement emails
│   │
│   ├── validation/                  # Zod request schemas
│   ├── tests/                       # Jest + Supertest tests
│   ├── db.js                        # MongoDB connection
│   ├── firebaseAdmin.js             # Firebase Admin SDK init
│   ├── index.js                     # Server entry point
│   ├── seed.js                      # Product seed script
│   ├── seedFitnessCenters.js        # Fitness center seed script
│   ├── Dockerfile                   # Node.js production image
│   └── package.json                 # Dependencies & scripts
│
├── docs/                            # ── DOCUMENTATION ───────────────
│   ├── CONTRIBUTING.md              #   Contribution guide
│   ├── FIRST_PURCHASE_EMAIL_SETUP.md #  Email setup instructions
│   ├── PRODUCTS_API.md              #   Products API documentation
│   └── SECURITY.md                  #   Security disclosure policy
│
├── docker-compose.yml               # Full-stack orchestration
├── LICENSE                          # MIT License
└── README.md                        # This file
```

---

## 🔗 File-to-File Handoff Chain

This section traces the exact sequence of files executed from startup to a running application — showing how control passes from one file to the next.

### Client Startup Chain

```mermaid
graph LR
    A["1. index.html"] -->|script tag| B["2. main.jsx"]
    B -->|imports CSS| C["3. index.css (Tailwind)"]
    B -->|renders| D["4. App.jsx"]
    D -->|wraps app in| E["5. ErrorBoundary.jsx"]
    E -->|provides| F["6. QueryClientProvider"]
    F -->|creates| G["7. BrowserRouter + Routes"]
    G -->|determines route| H["8. Page Component"]
    H -->|calls| I["9. useAuth() hook"]
    I -->|initializes| J["10. firebase.js"]
    J -->|connects to| K["Firebase Auth Service"]

    style A fill:#2d2d2d,color:#fff
    style D fill:#61DAFB,color:#000
    style J fill:#FFCA28,color:#000
    style K fill:#FF6D00,color:#fff
```

| Step | File | What Happens |
|------|------|-------------|
| 1 | `client/index.html` | Browser loads the HTML shell with `<div id="root">` and `<script src="/src/main.jsx">` |
| 2 | `client/src/main.jsx` | Vite processes the ESM import, React's `createRoot` mounts `<App />` into `#root` inside `<StrictMode>` |
| 3 | `client/src/index.css` | Tailwind CSS v4 is imported (`@import "tailwindcss"`) — all utility classes become available |
| 4 | `client/src/App.jsx` | Instantiates `QueryClient`, wraps the app in `ErrorBoundary` → `QueryClientProvider` → `BrowserRouter` |
| 5 | `client/src/App.jsx` | `<Routes>` evaluates the current URL and renders the matching page component |
| 6 | `components/NonAdminRoute.jsx` | Public routes are wrapped: checks if user is admin → redirects to `/admin/dashboard` if so |
| 7 | `components/AdminRoute.jsx` | Admin routes are wrapped: checks if user UID === `VITE_ADMIN_UID` → redirects to `/home` if not |
| 8 | `auth/useAuth.js` | Hook subscribes to `onAuthStateChanged(auth, callback)` — returns `{ user, loading }` |
| 9 | `auth/firebase.js` | `initializeApp(firebaseConfig)` using `VITE_FIREBASE_*` env vars → exports `auth` instance |
| 10 | `utils/getAuthHeaders.js` | For every API call: gets `user.getIdToken()` → returns `{ Authorization: "Bearer <token>" }` |

### Server Startup Chain

```mermaid
graph TD
    A["1. index.js (Entry)"] -->|require dotenv| B["2. .env loaded"]
    A -->|validate| C["3. Env var checks"]
    C -->|critical missing?| D["process.exit(1)"]
    C -->|all ok| E["4. Middleware setup"]
    E --> F["CORS + Helmet + Rate Limiter + JSON parser"]
    A -->|require| G["5. db.js"]
    G -->|mongoose.connect| H["MongoDB"]
    A -->|require| I["6. firebaseAdmin.js"]
    I -->|admin.initializeApp| J["Firebase Admin"]
    A -->|require| K["7. middleware/logger.js"]
    A -->|mount routes| L["8. routes/*.js"]
    L --> M["/api/products → routes/products.js"]
    L --> N["/api/cart → routes/cart.js"]
    L --> O["/api/payment → routes/payment.js"]
    L --> P["/api/chat → routes/chat.js"]
    L --> Q["/api/user → routes/user.js"]
    L --> R["... additional route modules"]
    A -->|app.listen| S["9. Server live on PORT"]

    style A fill:#339933,color:#fff
    style H fill:#47A248,color:#fff
    style J fill:#FFCA28,color:#000
    style S fill:#1a1a2e,color:#fff
```

| Step | File | What Happens |
|------|------|-------------|
| 1 | `server/index.js` | Entry point. Loads `dotenv`, creates `express()` app |
| 2 | `server/.env` | `dotenv.config()` reads all environment variables into `process.env` |
| 3 | `server/index.js` | Validates critical env vars (`MONGO_URI`) — exits if missing. Warns for optional vars |
| 4 | `server/index.js` | Stacks middleware: `cors()`, `helmet()`, `express.json()`, `rateLimit()`, cache-control headers |
| 5 | `server/db.js` | `mongoose.connect(MONGO_URI)` — connects to MongoDB Atlas or local instance |
| 6 | `server/firebaseAdmin.js` | Initializes Firebase Admin SDK with service account credentials (from env vars or JSON file) |
| 7 | `server/middleware/logger.js` | Custom colored request/response logger for dev visibility |
| 8 | `server/index.js` | Mounts all route modules under `/api/*` prefixes |
| 9 | `server/index.js` | `app.listen(port)` — server is now accepting HTTP requests |

### Docker Startup Chain

```mermaid
graph TD
    A["docker-compose.yml"] --> B["1. MongoDB (mongo:7)"]
    A --> R["Redis (redis:7)"]
    B -->|healthcheck passes| C["2. Server (Dockerfile)"]
    R -->|healthcheck passes| C
    C -->|healthcheck passes| D["3. Client (Dockerfile + Nginx)"]

    B --> E["Volume: mongodb_data"]
    C --> F["Reads: server/.env"]
    D --> G["Build args: VITE_* env vars"]

    subgraph "Network: fitmart-network (bridge)"
        B
        C
        D
        R
    end

    style A fill:#2496ED,color:#fff
    style B fill:#47A248,color:#fff
    style R fill:#DC382D,color:#fff
    style C fill:#339933,color:#fff
    style D fill:#61DAFB,color:#000
```

| Order | Service | Container | Port | Depends On |
|-------|---------|-----------|------|------------|
| 1 | MongoDB | `fitmart-mongodb` | `27017` | — |
| 1 | Redis | `fitmart-redis` | `6379` | — |
| 2 | Server (Express) | `fitmart-server` | `5000` | MongoDB ✅, Redis ✅ |
| 3 | Client (Nginx) | `fitmart-client` | `80` | Server ✅ |

---

## 🚀 How to Run the Project

### Prerequisites

Make sure you have the following installed:

- [Node.js](https://nodejs.org/) v16+
- [npm](https://www.npmjs.com/) or [yarn](https://yarnpkg.com/)
- A [MongoDB](https://www.mongodb.com/atlas) connection (Atlas or local)
- A [Firebase](https://firebase.google.com/) project (for auth)
- A [Razorpay](https://razorpay.com/) account (for payments)
- A [Google Gemini API key](https://ai.google.dev/) (for the AI chatbot)
- A [RapidAPI](https://rapidapi.com/justin-thewebdev/api/exercisedb) account with ExerciseDB access (for exercises)
- An SMTP provider (e.g., Gmail) for transactional emails *(optional)*


###  Environment Variables

> ⚠️ **Never commit your `.env` files or API secrets to GitHub!** They are already in `.gitignore`.

#### Server — `server/.env`

```env
# Required
MONGO_URI=<your_mongodb_connection_string>
PORT=5000

# Firebase Admin SDK (required for auth middleware)
FIREBASE_PROJECT_ID=<your_firebase_project_id>
FIREBASE_CLIENT_EMAIL=<your_firebase_client_email>
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"

# Admin UID
ADMIN_UID=<firebase_uid_of_admin_account>

# Payment processing
RAZORPAY_KEY_ID=<your_razorpay_key_id>
RAZORPAY_KEY_SECRET=<your_razorpay_key_secret>

# AI Chatbot — Google Gemini
GEMINI_API_KEY=<your_gemini_api_key>

# Exercise library — RapidAPI ExerciseDB
RAPIDAPI_KEY=<your_rapidapi_key>
RAPIDAPI_HOST=exercisedb.p.rapidapi.com

# CORS
ALLOWED_ORIGIN=http://localhost:5173

# Transactional email — SMTP (optional)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_SECURE=false
SMTP_USER=your-email@gmail.com
SMTP_PASS=your-app-password
SMTP_FROM=noreply@fitmart.com
APP_BASE_URL=http://localhost:5173
```

> **Startup behaviour:** `MONGO_URI` is the only truly critical variable — the server will exit if it's missing. All other variables are optional; missing ones produce a warning and disable the corresponding feature gracefully.

#### Getting Firebase Admin Credentials

1. Go to [Firebase Console](https://console.firebase.google.com) → **Project Settings** → **Service Accounts**
2. Select **Node.js** and click **"Generate new private key"**
3. Copy `project_id`, `client_email`, and `private_key` into the corresponding env vars
4. **Delete the downloaded `.json` file** — never commit it

#### Getting a Gemini API Key

1. Visit [Google AI Studio](https://aistudio.google.com/)
2. Sign in and click **"Get API key"**
3. Copy the key and add it as `GEMINI_API_KEY`

#### Client — `client/.env`

```env
VITE_API_URL=http://localhost:5000
VITE_RAZORPAY_KEY_ID=<your_razorpay_key_id>
VITE_ADMIN_UID=<firebase_uid_of_admin_account>

# Firebase config (from Firebase Console → Project Settings → General)
VITE_FIREBASE_API_KEY=
VITE_FIREBASE_AUTH_DOMAIN=
VITE_FIREBASE_PROJECT_ID=
VITE_FIREBASE_STORAGE_BUCKET=
VITE_FIREBASE_MESSAGING_SENDER_ID=
VITE_FIREBASE_APP_ID=
VITE_FIREBASE_MEASUREMENT_ID=
```

### 3. Seed the Database

```bash
cd server

# Seed products (equipment, nutrition, wearables)
npm run seed

# Seed fitness centers (optional)
npm run seed:fitness
```

### 4. Run in Development

```bash
# Terminal 1 — Backend
cd server && npm run dev
# → Server runs at http://localhost:5000

# Terminal 2 — Frontend
cd client && npm run dev
# → Client runs at http://localhost:5173
```

### 5. Run with Docker (Optional)

If you have [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed:

```bash
# Create server env file
cp server/.env.example server/.env
# (fill in your values)

# Build and start all services
docker compose up --build
```

| Service | URL |
|---------|-----|
| React client | http://localhost |
| Node API | http://localhost:5000 |
| MongoDB | mongodb://localhost:27017 |

```bash
docker compose down          # stop containers
docker compose down -v       # stop + delete the MongoDB volume
```

---

## 🗺️ User Journey Through the Project

### Journey 1 — New Visitor → First Purchase

```mermaid
graph TD
    A["🌐 User visits / (LandingPage)"] --> B["Sees hero, categories, testimonials"]
    B --> C["Clicks 'Shop Now' → /auth"]
    C --> D["Signs up via Email or Google"]
    D --> E["Redirected to /home (HomePage)"]
    E --> F["WelcomeBanner shows 10% discount"]
    E --> G["Browses product catalog"]
    G --> H["Clicks a product → /product/:id"]
    H --> I["Clicks 'Add to Cart'"]
    I --> J["CartDrawer slides in"]
    J --> K["Clicks 'Checkout' → /checkout"]
    K --> L["Reviews order + discount applied"]
    L --> M["Clicks 'Pay Now' → /payment"]
    M --> N{"Razorpay configured?"}
    N -->|Yes| O["Razorpay modal opens"]
    N -->|No| P["'Simulate Success' button"]
    O --> Q["Payment verified via HMAC"]
    P --> Q
    Q --> R["Order created + stock deducted"]
    R --> S["FitRewards points awarded"]
    R --> T["First-purchase email sent"]
    R --> U["Redirected to /payment-confirmation"]
    U --> V["🎉 Success screen with order details"]

    style A fill:#1a1a2e,color:#fff
    style V fill:#2ecc71,color:#fff
    style Q fill:#0C2A5E,color:#fff
```

**Files responsible for each step:**

| Step | Client File(s) | Server File(s) |
|------|----------------|----------------|
| Landing page | `pages/LandingPage.jsx` | — |
| Auth (sign up / sign in) | `pages/Authentication.jsx`, `auth/firebase.js` | `routes/user.js` (POST `/login`) |
| Product catalog | `pages/HomePage.jsx`, `hooks/useInfiniteProducts.js` | `routes/products.js` (GET) |
| Welcome discount banner | `components/WelcomeBanner.jsx`, `auth/useWelcomeDiscount.js` | `routes/user.js` (GET `/discount-status`) |
| Product detail | `pages/ProductPage.jsx` | `routes/products.js` (GET `/:id`) |
| Add to cart (reserve stock) | `components/CartDrawer.jsx` | `routes/cart.js` (POST `/:userId/add`) |
| Checkout review | `pages/Checkout.jsx` | `routes/cart.js` (GET `/:userId`) |
| Payment processing | `pages/PaymentPage.jsx` | `routes/payment.js` (POST `/create-order`, `/verify-payment`) |
| Order creation | — | `services/orderService.js`, `models/Order.js` |
| Loyalty points | — | `routes/payment.js`, `models/Rewards.js` |
| Welcome email | — | `services/firstPurchaseEmailService.js` |
| Confirmation screen | `pages/ProductConfirmation.jsx` | — |

### Journey 2 — Workout & Fitness Tracking

```mermaid
graph TD
    A["User logged in at /home"] --> B["Clicks 'Tracker' in Navbar"]
    B --> C["/tracker (TrackerPage)"]
    C --> D["WorkoutCalendar renders FullCalendar"]
    D --> E["User clicks a date"]
    E --> F["/notes?date=YYYY-MM-DD (NotesPage)"]
    F --> G["User logs exercises & notes"]
    G --> H["Data saved to localStorage"]

    A --> I["Clicks 'Exercises' in Navbar"]
    I --> J["/exercises (ExercisePage)"]
    J --> K["Selects muscle group (e.g., Chest)"]
    K --> L["API call to /api/exercises?category=chest"]
    L --> M["ExerciseDB returns exercises + GIFs"]
    M --> N["Exercises displayed with animations"]

    A --> O["Opens FitnessChatBot (FAB)"]
    O --> P["Types fitness question"]
    P --> Q["POST /api/chat → Gemini 2.5 Flash"]
    Q --> R["AI response rendered with markdown"]

    A --> S["Uses BMI Calculator widget"]
    S --> T["Computes BMI, BMR, TDEE"]
    T --> U["Shows product category recommendation"]

    style C fill:#e74c3c,color:#fff
    style J fill:#3498db,color:#fff
    style O fill:#9b59b6,color:#fff
    style S fill:#f39c12,color:#fff
```

**Files responsible:**

| Feature | Client File(s) | Server File(s) |
|---------|----------------|----------------|
| Workout Calendar | `pages/TrackerPage.jsx`, `components/WorkoutCalendar.jsx` | — (localStorage only) |
| Workout Notes | `pages/NotesPage.jsx`, `utils/workoutStorage.js` | — (localStorage only) |
| Exercise Browser | `pages/ExercisePage.jsx` | `routes/exercises.js` (proxies RapidAPI) |
| AI Chatbot | `components/FitnessChatBot.jsx` | `routes/chat.js`, `config/chatConfig.js` |
| BMI Calculator | `components/BMICalculator.jsx`, `utils/healthUtils.js` | — |
| Calorie Calculator | `components/CalorieCalculator.jsx`, `utils/healthUtils.js` | — |
| Nearby Fitness Centers | `components/NearbyFitnessCenters.jsx` | `routes/fitnessCenters.js` |

### Journey 3 — Admin Panel

```mermaid
graph TD
    A["Admin signs in (UID matches VITE_ADMIN_UID)"] --> B["NonAdminRoute detects admin"]
    B --> C["Redirected to /admin/dashboard"]
    C --> D["AdminNavbar renders"]
    C --> E["Dashboard KPIs, charts, recent orders"]

    D --> F["/admin/inventory"]
    F --> G["Stock levels, low-stock alerts"]

    D --> H["/admin/customers"]
    H --> I["Customer list with segments"]
    I --> J["/admin/customers/:userId"]
    J --> K["Customer detail + order history"]

    D --> L["/admin/reports"]
    L --> M["Sales reports: daily/weekly/monthly"]

    D --> N["/admin/marketing"]
    N --> O["Marketing strategy cards"]

    D --> P["/admin/bugs"]
    P --> Q["User-submitted bug reports"]
    Q --> R["Status: open → in-progress → resolved"]

    style C fill:#e74c3c,color:#fff
    style F fill:#f39c12,color:#fff
    style H fill:#3498db,color:#fff
    style L fill:#2ecc71,color:#fff
    style P fill:#9b59b6,color:#fff
```

**Files responsible:**

| Admin Page | Client File | Server Route |
|-----------|-------------|-------------|
| Dashboard | `pages/AdminDashboard.jsx` | `routes/dashboard.js` |
| Inventory | `pages/AdminInventory.jsx` | `routes/products.js` |
| Customers | `pages/AdminCustomers.jsx` | `routes/customers.js` |
| Customer Detail | `pages/AdminCustomerDetail.jsx` | `routes/customers.js` (GET `/:userId`) |
| Reports | `pages/AdminReports.jsx` | `routes/reports.js` |
| Marketing | `pages/AdminMarketing.jsx` | — (static content) |
| Bug Tracker | `pages/AdminBugs.jsx` | `routes/bugs.js` |

---

## 💡 Impact & What This Project Demonstrates

FitMart is a **production-grade, feature-rich MERN e-commerce platform** that demonstrates mastery of modern full-stack development through:

| # | Capability | What It Shows |
|---|-----------|--------------|
| 1 | **Full Authentication Flow** | Firebase Auth on client + Admin SDK verification on server — secure, industry-standard identity management |
| 2 | **Secure Payment Processing** | Razorpay integration with HMAC-SHA256 server-side verification — real-world payment gateway handling |
| 3 | **Real-Time Inventory Management** | Stock reservation system that prevents overselling during concurrent cart operations — solving a real e-commerce concurrency challenge |
| 4 | **AI Integration** | Google Gemini 2.5 Flash powers a contextual fitness chatbot — demonstrating practical generative AI implementation |
| 5 | **Comprehensive Admin Panel** | Dashboard analytics, inventory, customer segmentation, sales reports, marketing, and bug tracking — a complete business management tool |
| 6 | **Fitness Tracking Suite** | Calendar-based workout planner, exercise browser with GIF previews, BMI/calorie calculators — extending an e-commerce store into a fitness platform |
| 7 | **Email Automation** | Automated welcome emails and re-engagement campaigns — lifecycle marketing implementation |
| 8 | **Loyalty Program** | FitRewards points system — customer retention strategy |
| 9 | **Production-Ready Security** | Rate limiting, Helmet, CORS, Zod input validation, resource ownership guards — defense-in-depth approach |
| 10 | **Containerized Deployment** | Docker Compose for one-command full-stack startup with health checks and service ordering — modern DevOps practices |

> This project bridges the gap between tutorial-level apps and production software — demonstrating how real e-commerce platforms handle authentication, payments, inventory, and user engagement at scale.

---


---





# Fit_Mart
FitMart is a full-stack MERN fitness e-commerce platform with Firebase authentication, Razorpay payments, workout tracking, a Gemini AI fitness chatbot, an admin dashboard for sales and inventory, and Docker-based deployment.
