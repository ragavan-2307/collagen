# COLLAGEN AGE ANALYZER
## Understanding Collagen Changes in Aging Skin

A comprehensive, web-based educational application examining how chronological age and everyday lifestyle factors (UV exposure, sleep, hydration, nutrition, smoking, physical activity, and skincare) correlate with dermal extracellular matrix dynamics and collagen degradation.

---

## 1. Project Objective & Core Concept

**Collagen Age Analyzer** is an educational assessment platform that helps users understand:
- The biological function of Type I and Type III collagen fibers, elastin, and fibroblasts in skin.
- How natural chronological aging causes an intrinsic deceleration in collagen synthesis (~1% annually).
- How modifiable extrinsic factors (solar UV radiation, glycation from refined sugars, reactive oxygen species from smoke, and sleep deprivation) upregulate matrix metalloproteinases (MMPs) and accelerate collagen breakdown.
- Practical, evidence-informed lifestyle habits to support natural procollagen synthesis and epidermal barrier resilience.

> **Important Scientific Notice:** This application is strictly an educational tool. It does not measure physical collagen levels, biological skin age, or provide clinical medical diagnoses.

---

## 2. The Five Primary Sections

1. **HOME PAGE (`#home`)**
   - Hero introducing the project and scientific context.
   - Interactive anatomical cross-section SVG illustrating the Epidermis, Dermis, Fibroblasts, Elastin fibers, and Collagen bundles.
   - Core educational explanations of collagen biology.
   - Five-step workflow overview.

2. **USER ASSESSMENT (`#assessment`)**
   - 9-category interactive questionnaire:
     1. Age Range
     2. Sun Exposure
     3. Sun Protection
     4. Sleep Habits
     5. Hydration Habits
     6. Nutrition Patterns
     7. Physical Activity
     8. Smoking Exposure
     9. Skincare Habits
   - Visual progress bar, required validation, answer preservation, and pre-submission review screen.

3. **COLLAGEN ANALYSIS (`#analysis`)**
   - Transparent scoring engine view explaining factor weightings:
     - Photoprotection & UV (25%)
     - Sleep & Nocturnal Recovery (15%)
     - Nutrition & Glycation Defense (15%)
     - Smoke & Environmental Oxidants (15%)
     - Hydration & Extracellular Turgor (10%)
     - Physical Activity & Perfusion (10%)
     - Skincare & Barrier Maintenance (10%)

4. **RESULTS DASHBOARD (`#results`)**
   - Overall Educational Collagen Support Index (0 – 100).
   - Categorized status: High Support, Moderate Support, or Elevated Modifiable Risk.
   - Chronological age physiology context card.
   - Detailed factor cards with scores, status badges, and scientific rationales.
   - **Chart.js Visualizations:**
     - Horizontal Factor Bar Chart
     - Lifestyle Balance Doughnut Chart
     - User vs. Dermatological Benchmark Radar Chart
   - One-click **Download PDF Report** generating an official scientific assessment document.

5. **RECOMMENDATIONS (`#recommendations`)**
   - Tailored educational lifestyle guidance categorized into:
     - Sun Protection
     - Nutrition & Antioxidants
     - Hydration
     - Sleep & Circadian Rhythm
     - Physical Activity
     - Skincare & Barrier Integrity
   - Each includes category, scientific explanation, and practical daily actionable habit.

---

## 3. Technology Stack

- **Frontend:** HTML5, CSS3, Vanilla JavaScript, Chart.js (CDN).
- **Backend:** Node.js, Express.js, REST APIs, PDFKit.
- **Database:** PostgreSQL with Prisma ORM.
- **Testing:** Node.js native test runner (`node:test`, `node:assert`).

---

## 4. Directory Structure

```text
Collagen/
├── package.json                          # Root package and scripts
├── README.md                             # Project documentation
├── .env.example                          # Environment template
│
├── frontend/                             # Vanilla HTML5/CSS3/JS Frontend
│   ├── index.html                        # Main single-page application
│   ├── css/
│   │   ├── style.css                     # Design tokens & base styles
│   │   ├── components.css                # Buttons, cards, options, badges
│   │   └── responsive.css                # Mobile & tablet media queries
│   ├── js/
│   │   ├── api.js                        # REST API client
│   │   ├── validation.js                 # Input validation logic
│   │   ├── navigation.js                 # Section transitions & mobile menu
│   │   ├── questionnaire.js              # Step state & answer preservation
│   │   ├── assessment.js                 # Submission & analysis coordinator
│   │   ├── results.js                    # Results dashboard renderer
│   │   ├── charts.js                     # Chart.js visualization engine
│   │   ├── recommendations.js            # Guidance cards renderer
│   │   └── app.js                        # Application bootstrap
│   └── assets/
│       ├── images/
│       ├── icons/
│       └── illustrations/
│
└── backend/                              # Node.js & Express.js Backend
    ├── package.json                      # Backend dependencies
    ├── .env.example                      # Backend env template
    ├── prisma/
    │   ├── schema.prisma                 # PostgreSQL database models
    │   └── migrations/
    │       └── 20261121000000_init/
    │           └── migration.sql         # SQL schema migration
    ├── src/
    │   ├── server.js                     # Server entry point
    │   ├── app.js                        # Express app & static serving
    │   ├── config/
    │   │   ├── database.js               # Prisma & PostgreSQL connectivity
    │   │   └── environment.js            # Env configuration
    │   ├── controllers/
    │   │   ├── assessmentController.js   # Assessment handling
    │   │   ├── resultController.js       # Result retrieval
    │   │   ├── recommendationController.js# Recommendations retrieval
    │   │   └── reportController.js       # PDF generation & streaming
    │   ├── middleware/
    │   │   ├── errorHandler.js           # Centralized error handler
    │   │   └── validateRequest.js        # Request body validator
    │   ├── routes/
    │   │   ├── healthRoutes.js           # /api/health
    │   │   ├── assessmentRoutes.js       # /api/assessments
    │   │   ├── resultRoutes.js           # /api/results
    │   │   ├── recommendationRoutes.js   # /api/recommendations
    │   │   └── reportRoutes.js           # /api/reports
    │   ├── services/
    │   │   ├── scoringService.js         # Deterministic educational scoring engine
    │   │   ├── assessmentService.js      # Assessment workflow & persistence
    │   │   ├── recommendationService.js  # Approved educational content mapping
    │   │   └── reportService.js          # Publication-quality PDF generation
    │   ├── validators/
    │   │   └── assessmentValidators.js   # Payload validation schemas
    │   └── utils/
    │       ├── apiResponse.js            # Standardized REST responses
    │       └── logger.js                 # Structured logger
    └── tests/
        ├── scoring.test.js               # Unit tests for scoring & validation
        └── api.test.js                   # Integration tests for all endpoints
```

---

## 5. Getting Started & Installation

### Step 1: Install Dependencies
From the repository root:
```bash
cd backend
npm install
```

### Step 2: Configure Environment
Copy `.env.example` to `.env`:
```bash
cp .env.example .env
```
Ensure your configuration settings are defined:
```env
PORT=5000
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/collagen_db?schema=public"
```

### Step 3: PostgreSQL Database Configuration
1. Make sure a PostgreSQL database instance is running.
2. Create the target database:
   ```sql
   CREATE DATABASE collagen_db;
   ```
3. Run the Prisma migrations to create all required tables:
   ```bash
   cd backend
   npx prisma migrate dev --name init
   ```
4. If PostgreSQL is offline, the backend gracefully runs in **Educational Local Mode** (processing scoring and reports via in-memory caching) and reports database status on `/api/health`.

### Step 4: Run the Application
Start the backend server (which also serves the frontend):
```bash
cd backend
npm run dev
```

Open your browser and navigate to:
```
http://localhost:5000
```

---

## 6. Running Automated Tests

Run all unit and integration tests using Node's built-in test runner:
```bash
npm test
```
Or directly from the backend directory:
```bash
cd backend
node --test tests/scoring.test.js tests/api.test.js
```

---

## 7. REST API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/health` | Service health status and PostgreSQL connectivity |
| `POST` | `/api/assessments` | Submit questionnaire responses and calculate results |
| `GET` | `/api/assessments/:id` | Retrieve single assessment record |
| `GET` | `/api/results/:assessmentId` | Retrieve calculated score, factors, and age context |
| `GET` | `/api/recommendations/:assessmentId` | Retrieve tailored educational recommendations |
| `GET` | `/api/reports/assessments/:assessmentId` | Download formatted assessment PDF report |
| `GET` | `/` | Serves the HTML5/CSS3/Vanilla JS frontend |

---

## 8. Scientific & Educational Disclaimer

This application is designed solely for educational, informational, and self-reflection purposes. It does not provide medical diagnoses, treatment plans, prescriptions, or guarantees regarding skin health or collagen preservation. Dermal collagen density and biological skin age can only be evaluated by licensed medical professionals through clinical examination.
