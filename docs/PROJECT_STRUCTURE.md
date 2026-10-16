# 📂 Krishi Mitra — Verified Project Structure & Codebase Audit

> **Document Status**: [VERIFIED IMPLEMENTED]  
> **Audited Date**: October 2026  
> **Frameworks**: React 18 + Vite (Frontend) | FastAPI 0.111.0 + Uvicorn (Backend)  
> **Python Version**: Python 3.12 / 3.13 Verified  

---

## 1. Complete Verified Directory Tree

```text
Krishi-Mitra-1/
├── .gitignore                                  # Git exclusion rules (ignores models, datasets, venv)
├── package.json                                # Frontend dependencies and build scripts
├── package-lock.json                           # NPM dependency lockfile
├── vite.config.js                              # Vite bundler configuration with React plugin
├── eslint.config.js                            # ESLint 8.57 flat configuration
├── index.html                                  # Single Page Application HTML root
├── README.md                                   # Root overview and setup instructions
├── agents.md                                   # Coding standards and AI agent behavioral constraints
├── ML_IMPLEMENTATION_PLAN.md                   # Machine learning feature roadmap & status matrix
├── KRISHI_MITRA_ML_RESEARCH_AND_ANTIGRAVITY_PROMPT.md # Domain research, prompt specs, dataset URLs
│
├── docs/                                       # Official Comprehensive Project Documentation
│   ├── README.md                               # Master Documentation Hub & Presentation Index
│   ├── PROJECT_OVERVIEW.md                     # System overview, problem statement, stakeholders
│   ├── PROJECT_STRUCTURE.md                    # This verified file & component audit
│   ├── IMPLEMENTATION_STATUS.md                # Feature verification matrix
│   ├── DATASET_AUDIT.md                        # Complete audit of PlantVillage, Crop Rec, Mandi data
│   ├── GEOGRAPHIC_COVERAGE.md                  # State & district coverage, UP/Jalaun pilot analysis
│   ├── MARKETPLACE_AND_ORDER_FLOW.md           # Mandi info dashboard vs transactional marketplace
│   ├── FARMER_SCHEMES_AND_SOURCES.md           # Central & State scheme directory & portal links
│   ├── API_AND_ENVIRONMENT_AUDIT.md            # External integrations, keys, security & quotas
│   ├── WEATHER_SYSTEM.md                       # Agro-weather pipeline, caching, 503 root cause
│   ├── AI_AGENT_ARCHITECTURE.md                # Gemini AI agent, rate limits, multi-agent design
│   ├── MODEL_TRAINING_AND_EVALUATION.md        # Reproducible ML training, metrics, architectures
│   ├── PROJECT_PRESENTATION_NOTES.md           # Viva, evaluator defense, demo scripts
│   └── flowcharts/                             # Mermaid Architecture & Logic Diagrams
│       ├── system_architecture.mmd             # End-to-end system topology
│       ├── plant_disease_detection.mmd         # Leaf scanning, CNN inference & GenAI advisory
│       ├── crop_recommendation.mmd             # Soil/weather inputs, scaling & Random Forest
│       ├── mandi_price_pipeline.mmd            # Spot prices & econometric forecasting
│       ├── weather_pipeline.mmd                # Weather proxy, 503 handling & advisory rules
│       ├── farmer_scheme_discovery.mmd         # Scheme filtering & official portal redirection
│       ├── farmer_marketplace_orders.mmd       # Current info dashboard vs proposed marketplace
│       └── complete_farmer_user_journey.mmd    # End-to-end user journey across all modules
│
├── krishi-backend/                             # FastAPI Machine Learning Backend
│   ├── main.py                                 # Core API server, routing, GenAI & ML glue code
│   ├── requirements.txt                        # Backend Python dependencies
│   ├── env.example                             # Environment variable template with documentation
│   ├── .env                                    # Local active environment variables (untracked)
│   ├── crop_recommendation/                    # ML Model 2: Tabular Crop Recommendation
│   │   ├── __init__.py                         # Module export definition
│   │   ├── train.py                            # Reproducible Random Forest training pipeline
│   │   └── inference.py                        # Model artifact loader, input validator, inference
│   ├── disease_detection/                      # ML Model 1: Plant Disease Detection (MobileNetV2)
│   │   ├── __init__.py                         # Module export definition
│   │   ├── labels.json                         # 38 PlantVillage crop-pathogen category mappings
│   │   ├── metadata.json                       # Input dimensions (224x224x3), normalization, limits
│   │   ├── train.py                            # Transfer learning training pipeline
│   │   └── evaluate.py                         # Test evaluation (accuracy, F1, confusion matrix)
│   ├── mandi_forecast/                         # ML Model 3: Mandi Price Forecasting
│   │   ├── __init__.py                         # Module export definition
│   │   ├── ingestion.py                        # Agmarknet schema normalization, cleaning, dedup
│   │   ├── forecasting.py                      # Chronological split, Seasonal Naive & Lag Ridge
│   │   └── service.py                          # High-level coordinator & anti-fabrication guard
│   ├── datasets/                               # Training Data Directory
│   │   └── crop_recommendation/
│   │       └── Crop_recommendation.csv         # 2,200 rows, 7 features + 1 label, CC BY 3.0 IGO
│   ├── model/                                  # Trained Neural Network Artifacts
│   │   ├── krishi_model.h5                     # Keras HDF5 binary (10.1 MB, 38 classes)
│   │   ├── krishi_model.tflite                 # Edge-quantized TFLite binary (9.0 MB)
│   │   └── main.jsx                            # [ANOMALY] Stray 268-byte frontend file (benign)
│   ├── models/                                 # Trained Scikit-Learn Model Artifacts
│   │   └── crop_recommendation/
│   │       ├── crop_rf_model.joblib            # Trained Random Forest (200 trees, 7.2 MB)
│   │       ├── crop_scaler.joblib              # Fitted StandardScaler (735 bytes)
│   │       ├── crop_label_encoder.joblib       # Fitted LabelEncoder (696 bytes)
│   │       └── metadata.json                   # 99.55% accuracy metrics & training configuration
│   └── tests/                                  # Comprehensive Automated Test Suite
│       ├── __init__.py                         # Test package marker
│       └── test_backend.py                     # 41 unit/integration tests (all 41 passing)
│
└── src/                                        # React Single Page Application (SPA)
    ├── main.jsx                                # React application mounting & DOM rendering
    ├── App.jsx                                 # Application routing & context provider wrapping
    ├── index.css                               # Base typography, font imports, reset styles
    ├── App.css                                 # Design tokens, color system, grid utilities
    ├── layout-fixes.css                        # Responsive viewport padding & alignment overrides
    ├── components/                             # Reusable UI Components
    │   ├── AdvancedFeatures.jsx                # Extended feature highlights
    │   ├── DiseaseResultCard.jsx               # Active leaf diagnosis result, 48h action, gauge
    │   ├── DiseaseResultCard.component.jsx     # [DUPLICATE] Variant component of DiseaseResultCard
    │   ├── FarmerHelper.jsx                    # Interactive question bubble suggestions
    │   ├── Features.jsx                        # Homepage 6-card feature grid
    │   ├── Footer.jsx                          # App footer with copyright & links
    │   ├── Header.jsx                          # Top navigation header with auth links
    │   ├── Hero.jsx                            # Homepage banner, quick action CTAs
    │   ├── HowItWorks.jsx                      # 3-step farmer visual guide
    │   ├── MobileBottomNav.jsx                 # Bottom navigation bar for mobile devices
    │   ├── ProtectedRoute.jsx                  # Client-side authentication route guard
    │   ├── QueryInterface.jsx                  # AI query text input box
    │   ├── ResponseFormatter.jsx               # Markdown & structured advisory formatter
    │   ├── ResponseSection.jsx                 # Sample query response container
    │   ├── SkeletonLoader.jsx                  # Shimmer placeholders for async states
    │   ├── TopStrip.jsx                        # Top alert bar, language selector & emergency link
    │   └── WeatherWidget.jsx                   # Compact weather widget
    ├── context/                                # Global State Contexts
    │   ├── AuthContext.jsx                     # Mock authentication & LocalStorage manager
    │   └── LanguageContext.jsx                 # Localization dictionary (English, Hindi, Malayalam)
    ├── data/                                   # Static Agricultural Datasets
    │   └── schemes.js                          # Database of 10 Central & State farmer schemes
    ├── hooks/                                  # Custom React Hooks
    │   ├── useDiseaseScan.js                   # Leaf upload, state transitions, API bridge
    │   ├── useMarketPrices.js                  # Mandi spot price fetcher hook
    │   └── useWeather.js                       # Agro-weather fetcher hook (10-min interval)
    ├── pages/                                  # Primary Application Views
    │   ├── AdvisoriesPage.jsx                  # Static crop-season advisories
    │   ├── ChatPage.jsx                        # AI Farmer Assistant chat & leaf scanner
    │   ├── ChatPage.patch.jsx                  # [UNLINKED] Experimental patch file for ChatPage
    │   ├── ContactPage.jsx                     # KVK directory, expert helpline directory
    │   ├── CropRecommendationPage.jsx          # Tabular ML crop recommender UI
    │   ├── HomePage.jsx                        # Landing view container
    │   ├── LoginPage.jsx                       # Farmer login & sign-up modal
    │   ├── MarketPricePage.jsx                 # Mandi spot prices & price forecasting UI
    │   ├── ProfilePage.jsx                     # Farmer farm profile & crop preferences
    │   ├── SchemesPage.jsx                     # Searchable, filterable government scheme directory
    │   └── WeatherPage.jsx                     # Hyperlocal agro-weather & risk alerts
    ├── services/                               # Backend API Service Abstractions
    │   ├── cropService.js                      # POST /api/crop-recommendation client
    │   ├── diseaseService.js                   # POST /analyze & /advisory client
    │   ├── geminiService.js                    # POST /api/chat AI client
    │   ├── marketService.js                    # GET /api/mandi & POST /api/mandi/forecast client
    │   ├── voiceService.js                     # Web Speech API (SpeechRecognition & SpeechSynthesis)
    │   └── weatherService.js                   # GET /api/weather client & farming advisory rules
    ├── styles/                                 # Component-Specific CSS
    │   ├── ChatPage.css                        # Chat bubble and audio visualizer styles
    │   ├── ProfilePage.css                     # Farm profile card styles
    │   └── SchemesPage.css                     # Scheme card grid & filter styles
    └── utils/                                  # Frontend Helpers
        └── corsProxy.js                        # Client-side CORS helper utility
```

---

## 2. Frontend Framework & Entry Points

- **Framework**: React 18.2.0 bundled with Vite 5.2.0.
- **Routing**: `react-router-dom` v6.26.0 with client-side SPA routing.
- **Entry Points**:
  - `index.html`: Contains HTML shell, Google Fonts preconnect (`Inter`), and root DOM node `<div id="root"></div>`.
  - `src/main.jsx`: Mounts React application into `root` element.
  - `src/App.jsx`: Configures top-level providers (`LanguageProvider`, `AuthProvider`), sticky navigation layout (`TopStrip`, `Header`, `Footer`, `MobileBottomNav`), and client-side page routes.

### Active Frontend Routes (`src/App.jsx`)
| URL Path | Component | Description |
| :--- | :--- | :--- |
| `/` | `HomePage.jsx` | Landing page with hero banner, service grid, how-it-works, and footer |
| `/login` | `LoginPage.jsx` | Farmer authentication interface (email/password & simulated Google sign-in) |
| `/chat` | `ChatPage.jsx` | Multilingual AI advisory chat & leaf disease scanner |
| `/crop-recommendation` | `CropRecommendationPage.jsx` | Tabular ML crop recommender with soil presets and confidence gauges |
| `/market-prices` | `MarketPricePage.jsx` | APMC mandi spot prices & econometric price forecasting tabs |
| `/weather` | `WeatherPage.jsx` | 5-day weather forecast, hourly timeline & agricultural spraying alerts |
| `/schemes` | `SchemesPage.jsx` | Searchable government agricultural schemes directory |
| `/advisories` | `AdvisoriesPage.jsx` | Agronomic pest and weather advisories |
| `/profile` | `ProfilePage.jsx` | User profile, saved farm state, land size, and active crops |
| `/contact` | `ContactPage.jsx` | Krishi Vigyan Kendra (KVK) helpline numbers and support directory |

---

## 3. Backend Framework, Entry Points & Route Mapping

- **Framework**: FastAPI 0.111.0 running on Uvicorn ASGI server (`PORT=8000`).
- **Entry Point**: `krishi-backend/main.py`.
- **Startup Command**: `uvicorn main:app --host 0.0.0.0 --port 8000 --reload`.

### Complete Verified Backend Route Table
| HTTP Method | Route Path | Controller Function | Inputs / Parameters | Response Payload | Backing Engine |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `GET` | `/health` | `health()` | None | System status, mock mode flag, AI calls/day quota, service availability flags | Core Backend |
| `GET` | `/classes` | `get_classes()` | None | Count (38) and array of PlantVillage class objects with crop, disease, and healthy flag | Static JSON Array |
| `POST` | `/analyze` | `analyze_image()` | Multipart: `file` (image bytes), `region`, `season`, `language` | `raw_class`, `disease`, `crop`, `confidence`, `severity`, `advisory_short`, `advisory_detail`, `economic_impact`, `weather_advisory`, `mandi_advisory` | MobileNetV2 + Gemini GenAI |
| `POST` | `/advisory` | `get_advisory()` | JSON: `disease_name`, `confidence`, `crop`, `region`, `season`, `language`, `mode` | `disease`, `crop`, `language`, `mode`, `advisory`, `severity` | Gemini GenAI + Static Fallback |
| `POST` | `/api/chat` | `chat_endpoint()` | JSON: `question`, `language`, `state`, `weather_summary`, `active_crops` | `response`, `timestamp`, `fallback` flag | Gemini GenAI Proxy |
| `GET` | `/api/weather` | `weather_endpoint()` | Query: `lat`, `lon` (or `city`) | Temperature, feels_like, humidity, windSpeed, hourlyForecast, weeklyForecast | OpenWeatherMap API + In-Memory Cache |
| `GET` | `/api/mandi` | `mandi_endpoint()` | Query: `state`, `commodity`, `limit` (default 20) | Array of mandis with minPrice, maxPrice, modalPrice, price trend, date, source | Agmarknet API / State Mock Fallback |
| `POST` | `/api/mandi/forecast` | `mandi_forecast_endpoint()` | JSON: `state`, `commodity`, `market`, `horizon_days` (1–30) | Status, forecast projections, model_used, evaluation_metrics_held_out | LagRidgeForecaster / SeasonalNaive |
| `GET` | `/api/mandi/forecast/status` | `mandi_forecast_status()` | None | Readiness status, dataset presence, catalog URL, observation threshold (30 days) | Mandi Service |
| `POST` | `/api/crop-recommendation` | `crop_recommendation_endpoint()` | JSON: `N`, `P`, `K`, `temperature`, `humidity`, `ph`, `rainfall` | `recommended_crop`, `confidence`, `top_alternatives`, `feature_values`, `disclaimer` | Random Forest ML (200 Trees) |
| `GET` | `/api/crop-recommendation/info` | `crop_recommendation_info()` | None | Model metadata, feature list, 22 class names, accuracy metrics, limitations | Crop Rec Metadata |

---

## 4. Code Quality, Redundancies & Disconnected Modules

During this comprehensive code audit, the following specific files were identified and cataloged:

1. **`src/pages/ChatPage.patch.jsx` [UNUSED/UNLINKED]**:
   - A 11,139-byte patch file created during an earlier refactoring of `ChatPage.jsx`.
   - **Finding**: It is not imported by `App.jsx` or any other file. The application cleanly uses `src/pages/ChatPage.jsx` (25,369 bytes).
2. **`src/components/DiseaseResultCard.component.jsx` [DUPLICATE/UNUSED]**:
   - A 24,148-byte component file.
   - **Finding**: `src/pages/ChatPage.jsx` explicitly imports from `src/components/DiseaseResultCard.jsx` (30,503 bytes). `DiseaseResultCard.component.jsx` is an unlinked duplicate.
3. **`krishi-backend/model/main.jsx` [MISPLACED FILE]**:
   - A 268-byte copy of the frontend React root entry file located inside the backend model weights directory `krishi-backend/model/`.
   - **Finding**: Has no operational effect on the backend, but should be removed in future repo cleanups.
4. **`AuthContext.jsx` [MOCKED/CLIENT-SIDE ONLY]**:
   - `login()`, `loginWithGoogle()`, and `signup()` simulate authentication by writing mock JSON directly to browser `localStorage`.
   - **Finding**: No backend endpoints exist for user registration, password hashing, or JWT token verification.
5. **`AdvisoriesPage.jsx` [STATIC CONTENT]**:
   - Contains 5 hardcoded advisories primarily tailored to Kerala districts citing Kerala Agricultural University.
   - **Finding**: Not dynamically generated from backend or live ICAR feeds.
