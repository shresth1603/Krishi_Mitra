# 🌾 Krishi Mitra (कृषि मित्र) — Complete Project Overview

> **Document Status**: [VERIFIED IMPLEMENTED]  
> **Audience**: College Faculty, Academic Evaluators, Technical Mentors, Teammates, Agritech Investors  
> **Repository**: [https://github.com/shresth1603/Krishi-Mitra-1](https://github.com/shresth1603/Krishi-Mitra-1)  
> **Workspace Path**: `c:\Users\DELL\Krishi-Mitra-1`  
> **Current Version**: 3.0.0 (Production Architecture)

---

## 1. Executive Summary

**Krishi Mitra** (*Friend of the Farmer*) is an artificial-intelligence-driven agricultural decision-support platform designed to address the foundational challenges faced by Indian smallholder farmers:
1. **Delayed Crop Disease Diagnosis**: Up to 35% of Indian crop yield is lost annually to fungal, bacterial, and viral plant pathogens due to delayed diagnosis and generic chemical overuse.
2. **Suboptimal Crop Selection**: Farmers frequently plant traditional crops without evaluating soil macro-nutrients (Nitrogen, Phosphorus, Potassium), soil pH, and seasonal rainfall suitability.
3. **Mandi Price Asymmetry**: Middlemen and commission agents exploit lack of transparency in APMC (Agricultural Produce Market Committee) daily wholesale mandi prices.
4. **Hyperlocal Weather Vulnerability**: Sudden rains, high humidity, or severe wind drifts ruin chemical spraying operations and trigger severe blight outbreaks.
5. **Scheme Awareness Gaps**: Thousands of crores allocated under Central (PM-KISAN, PMFBY, KCC) and State farmer welfare schemes remain unclaimed due to fragmented documentation and eligibility confusion.

Krishi Mitra unifies computer vision, tabular machine learning, econometric time-series analysis, and generative AI into a cohesive, multilingual mobile-first web ecosystem.

---

## 2. Core Capabilities Matrix

| Functional Module | Technical Implementation | Backing Data / ML Engine | Verification Status | Source Reference |
| :--- | :--- | :--- | :--- | :--- |
| **Plant Disease Detection** | Upload leaf photo (JPEG/PNG) to predict pathogen & crop health | MobileNetV2 CNN transfer learning (38 classes, 224×224×3 RGB input) | `[VERIFIED IMPLEMENTED]` | [main.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L734-L819), [krishi_model.h5](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/model/krishi_model.h5) |
| **Agronomic 48-Hour Recovery Plan** | Structured chemical & organic spray protocols | Gemini GenAI API (google-genai SDK) with curated rule-based static fallback | `[VERIFIED IMPLEMENTED]` | [main.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L254-L300), [DiseaseResultCard.jsx](file:///c:/Users/DELL/Krishi-Mitra-1/src/components/DiseaseResultCard.jsx) |
| **Soil & Climate Crop Recommendation** | Multi-class crop ranking from soil NPK, pH, and climate | Random Forest Classifier (200 trees, 7 features, 22 crops) | `[VERIFIED IMPLEMENTED]` | [train.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/crop_recommendation/train.py), [inference.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/crop_recommendation/inference.py) |
| **APMC Mandi Spot Prices** | Daily wholesale minimum, maximum, and modal prices across mandis | Official Open Government Data Platform India (data.gov.in / AGMARKNET API) | `[VERIFIED IMPLEMENTED]` | [main.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L1040-L1150), [marketService.js](file:///c:/Users/DELL/Krishi-Mitra-1/src/services/marketService.js) |
| **Econometric Price Forecasting** | Multi-step forward price trend projection | Seasonal Naive baseline & Lag-Feature Ridge regression (strict chronological split) | `[VERIFIED IMPLEMENTED]` *(Engine verified; data archive pending)* | [forecasting.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/mandi_forecast/forecasting.py), [service.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/mandi_forecast/service.py) |
| **Agro-Weather Risk Engine** | 5-day forecasts & spray/irrigation windows | OpenWeatherMap API proxy + deterministic agricultural decision rules | `[VERIFIED IMPLEMENTED]` | [weatherService.js](file:///c:/Users/DELL/Krishi-Mitra-1/src/services/weatherService.js#L128-L166), [main.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L906-L1003) |
| **Government Schemes Discovery** | Filterable scheme directory by state, crop, and category | Curated database of 10 major Central & State schemes with direct links | `[VERIFIED IMPLEMENTED]` | [schemes.js](file:///c:/Users/DELL/Krishi-Mitra-1/src/data/schemes.js), [SchemesPage.jsx](file:///c:/Users/DELL/Krishi-Mitra-1/src/pages/SchemesPage.jsx) |
| **Multilingual Voice/Text Chatbot** | Speech-to-Text & Text-to-Speech agricultural advisor | Google GenAI chat proxy with state/weather context injection | `[VERIFIED IMPLEMENTED]` | [geminiService.js](file:///c:/Users/DELL/Krishi-Mitra-1/src/services/geminiService.js), [ChatPage.jsx](file:///c:/Users/DELL/Krishi-Mitra-1/src/pages/ChatPage.jsx) |
| **Transactional Marketplace** | Farmer listings, buyer cart, payments, delivery escrow | N/A — No e-commerce or checkout system exists | `[PLANNED]` | [MARKETPLACE_AND_ORDER_FLOW.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/MARKETPLACE_AND_ORDER_FLOW.md) |
| **User Authentication** | User sign-in, profile persistence | Browser LocalStorage simulation (no backend JWT/database) | `[MOCKED]` | [AuthContext.jsx](file:///c:/Users/DELL/Krishi-Mitra-1/src/context/AuthContext.jsx) |

---

## 3. System Architecture & High-Level Topology

```
┌────────────────────────────────────────────────────────────────────────┐
│                        FARMER CLIENT (SPA)                             │
│       React 18 + Vite | Vanilla CSS Design System | Responsive         │
│  - Leaf Scanner UI       - Crop Recommender Form  - Spot Mandi View    │
│  - AI Voice/Text Chat    - Agro-Weather Dashboard - Schemes Directory  │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │ HTTP / JSON / Multipart Form
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│                   BACKEND SERVICE LAYER (FastAPI :8000)                │
│  - CORS Middleware (Localhost & Production Whitelist)                  │
│  - In-Memory Sliding-Window Rate Limiter (12 RPM / 18 RPD)             │
│  - In-Memory Response Cache (300-second TTL)                           │
│  - Input Data Validation (Pydantic models, Pillow Image.verify)        │
└───────┬───────────────────────────┬────────────────────────────┬───────┘
        │                           │                            │
        ▼                           ▼                            ▼
┌──────────────────┐      ┌────────────────────┐      ┌──────────────────┐
│   ML ENGINES     │      │   EXTERNAL APIS    │      │  GENAI AGENT     │
│ 1. MobileNetV2   │      │ 1. OpenWeatherMap  │      │ 1. google-genai  │
│    (38 Classes)  │      │    (Proxy Key)     │      │    SDK Client    │
│ 2. Random Forest │      │ 2. data.gov.in     │      │ 2. Fallback Chain│
│    (22 Crops)    │      │    (Agmarknet API) │      │ 3. Static Expert │
│ 3. Lag-Ridge     │      └────────────────────┘      │    Rule Base     │
│    Time-Series   │                                  └──────────────────┘
└──────────────────┘
```

---

## 4. Key Stakeholders & Target Personas

1. **Small & Marginal Farmers (Landholding < 2 Hectares)**:
   - Need instant, vernacular (Hindi/Malayalam/English) leaf diagnosis without traveling 30km to district headquarters.
   - Benefit from zero-cost organic alternatives (Neem oil, cow urine/baking soda sprays) alongside generic chemicals.
2. **Krishi Vigyan Kendra (KVK) & Extension Officers**:
   - Platform acts as a first-line triaging tool; severe outbreaks (Late Blight, Yellow Leaf Curl) display explicit escalation alerts with KVK toll-free helpline guidance.
3. **Academic Evaluators & Project Reviewers**:
   - Demonstrates rigorous software engineering: clean separation of concerns, test coverage (41 backend unit tests), anti-fabrication data policies, and reproducible ML pipelines.
4. **Agritech Investors & Incubators**:
   - Scalable microservice design ready for containerization (Docker) and cloud deployment (Vercel frontend, AWS/GCP backend).

---

## 5. Development History & Git Evolution

Audit of Git commit trajectory reveals four major development phases:
1. **Foundation (`0735ec4`, `0ba1e61`)**: Initial React frontend layout, navigation components, and prototype FastAPI server structure.
2. **Security & Dependencies (`d8110e3`, `3f56ee8`, `f09f1f0`)**: Resolved environment variable leaking, moved Gemini API keys server-side, upgraded Pillow to 10.4.0 for secure image parsing.
3. **Model & Architecture Hardening (`dd73ec4`, `9e4a67a`)**: Integrated MobileNetV2 `krishi_model.h5` weights, corrupt byte handling, and Gemini SDK migration to `google-genai`.
4. **ML Expansion (`f3f7a22`)**: Added Random Forest crop recommendation (Model 2) with 99.55% accuracy, econometric time-series mandi forecasting engine (Model 3), and comprehensive Pytest test suite (41 tests).
