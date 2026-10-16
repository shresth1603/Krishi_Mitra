# 📚 Krishi Mitra (कृषि मित्र) — Complete Technical Documentation Hub

[![Live Demo](https://img.shields.io/badge/🌐_Live_Demo-krishi--mitra--nu.vercel.app-00C853?style=for-the-badge&logo=vercel&logoColor=white)](https://krishi-mitra-nu.vercel.app/)
[![GitHub Repository](https://img.shields.io/badge/GitHub-Repository-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/shresth1603/Krishi_Mitra)

> 🌐 **Live Web Application**: **[https://krishi-mitra-nu.vercel.app/](https://krishi-mitra-nu.vercel.app/)**  
> *Evaluators, mentors, and reviewers can test and navigate the live platform directly in the browser.*

Welcome to the official technical and functional documentation package for **Krishi Mitra-1**.

This documentation suite provides a complete, evidence-based audit of the entire codebase, datasets, machine learning models, external API integrations, system architectures, and user journeys. It is designed to be fully accessible and presentation-ready for **college faculty, project evaluators, technical mentors, teammates, and startup investors**.

---

## 🧭 Documentation Roadmap (Recommended Reading Order)

For an evaluator or reviewer inspecting the platform, follow this sequential reading guide:

```
  1. PROJECT OVERVIEW ──────────► Problem Statement, Stakeholders & High-Level Architecture
          │
          ▼
  2. PROJECT STRUCTURE ─────────► Complete Verified Codebase Tree, Endpoints & Component Map
          │
          ▼
  3. IMPLEMENTATION STATUS ─────► Feature Verification Matrix & Pytest Audit (41/41 Passing)
          │
          ▼
  4. DATASET AUDIT ─────────────► PlantVillage, Crop Recommendation & AGMARKNET Provenance
          │
          ▼
  5. MODEL TRAINING & EVAL ─────► MobileNetV2, Random Forest (99.55%), Lag-Ridge Math
          │
          ▼
  6. GEOGRAPHIC COVERAGE ───────► Real Data Coverage vs Selectors (UP/Jalaun Pilot Analysis)
          │
          ▼
  7. MARKETPLACE & ORDER FLOW ──► Mandi Info Service vs Transactional E-Commerce Roadmap
          │
          ▼
  8. WEATHER SYSTEM ────────────► Agro-Weather Pipeline, 503 Diagnosis & Spray Window Rules
          │
          ▼
  9. AI AGENT ARCHITECTURE ─────► Google GenAI Agent, Prompt Contracts & Rate Limiting
          │
          ▼
 10. FARMER SCHEMES & SOURCES ──► Central & UP State Schemes, Official Portals & DPDP Compliance
          │
          ▼
 11. API & ENVIRONMENT AUDIT ───► Security Audit, Secret Protection & Quota Limits
          │
          ▼
 12. FLOWCHARTS GALLERY ────────► 8 Mermaid Diagrams Visualizing Architecture & Pipelines
          │
          ▼
 13. PRESENTATION NOTES ────────► Academic Viva Defense, Evaluator Q&A, Live Demo Script
```

---

## 📖 Document Index & Descriptions

| Document | Path | Core Topics & Key Evidence |
| :--- | :--- | :--- |
| **Project Overview** | [PROJECT_OVERVIEW.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/PROJECT_OVERVIEW.md) | Problem statement in Indian agriculture, core feature summary, target personas, multi-tier architecture, and Git commit evolution. |
| **Project Structure** | [PROJECT_STRUCTURE.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/PROJECT_STRUCTURE.md) | Verified file tree, entry points (`index.html`, `main.py`), 11 backend routes, 10 frontend pages, and catalog of unused/duplicate files. |
| **Implementation Status** | [IMPLEMENTATION_STATUS.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/IMPLEMENTATION_STATUS.md) | Standardized status taxonomy (`[VERIFIED IMPLEMENTED]`, `[PARTIALLY IMPLEMENTED]`, `[PLANNED]`, `[MOCKED]`). 41/41 passing unit tests audit. |
| **Dataset Audit** | [DATASET_AUDIT.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/DATASET_AUDIT.md) | Deep audit of PlantVillage (38 classes, 14 crops), Crop Recommendation CSV (2,200 rows, 22 crops, CC BY 3.0 IGO), and AGMARKNET daily spot prices. |
| **Model Training & Evaluation** | [MODEL_TRAINING_AND_EVALUATION.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/MODEL_TRAINING_AND_EVALUATION.md) | MobileNetV2 CNN transfer learning, Random Forest 200 trees (99.55% accuracy, 99.59% 5-fold CV), Lag-Ridge auto-regression, and evaluation metrics. |
| **Geographic Coverage** | [GEOGRAPHIC_COVERAGE.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/GEOGRAPHIC_COVERAGE.md) | Real data coverage vs selectable dropdowns. Detailed Uttar Pradesh, Bundelkhand, and Jalaun (Orai) pilot deployment strategy. |
| **Marketplace & Order Flow** | [MARKETPLACE_AND_ORDER_FLOW.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/MARKETPLACE_AND_ORDER_FLOW.md) | Critical distinction between Mandi Price Information and transactional marketplaces. Analysis of B2C, B2B, C2C, F2C, F2B, B2B2C, and 4-phase roadmap. |
| **Weather System** | [WEATHER_SYSTEM.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/WEATHER_SYSTEM.md) | OpenWeatherMap proxy trace, 5-minute client/server caching, deterministic spray & fungal risk advisory rules, and root-cause diagnosis of 503 error. |
| **AI Agent Architecture** | [AI_AGENT_ARCHITECTURE.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/AI_AGENT_ARCHITECTURE.md) | Google GenAI SDK integration, 12 RPM / 18 RPD sliding-window rate limiter, model fallback chain, static fallback, and future multi-agent design. |
| **Farmer Schemes & Sources** | [FARMER_SCHEMES_AND_SOURCES.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/FARMER_SCHEMES_AND_SOURCES.md) | Official portal directory (PM-KISAN, PMFBY, KCC, Soil Health Card, UP State Subsidies), eligibility criteria, required documents, and DPDP Act compliance. |
| **API & Environment Audit** | [API_AND_ENVIRONMENT_AUDIT.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/API_AND_ENVIRONMENT_AUDIT.md) | Zero-secret exposure audit, environment variable matrix, backend proxy encapsulation, and security hygiene rules. |
| **Presentation Notes** | [PROJECT_PRESENTATION_NOTES.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/PROJECT_PRESENTATION_NOTES.md) | 2-minute elevator pitch, step-by-step live demo script, 6 common evaluator viva defense questions, and presentation slide metrics. |

---

## 📊 System Flowcharts Gallery

All architecture and process flowcharts are authored in standardized Mermaid format in [docs/flowcharts/](file:///c:/Users/DELL/Krishi-Mitra-1/docs/flowcharts/):

1. **[System Architecture](file:///c:/Users/DELL/Krishi-Mitra-1/docs/flowcharts/system_architecture.mmd)**: Main end-to-end multi-tier topology (Farmer Client -> FastAPI Gateway -> ML Engines / GenAI Agent / External APIs -> Response Validation -> UI).
2. **[Plant Disease Detection](file:///c:/Users/DELL/Krishi-Mitra-1/docs/flowcharts/plant_disease_detection.mmd)**: Leaf image validation, Pillow header check, 224×224×3 tensor preprocessing, MobileNetV2 inference, agronomic severity calculation, and Gemini recovery plan generation.
3. **[Crop Recommendation](file:///c:/Users/DELL/Krishi-Mitra-1/docs/flowcharts/crop_recommendation.mmd)**: Soil NPK, pH, and climate parameter validation, StandardScaler normalization, Random Forest inference (200 trees), and ranked top-5 candidate output.
4. **[Mandi Price Pipeline](file:///c:/Users/DELL/Krishi-Mitra-1/docs/flowcharts/mandi_price_pipeline.mmd)**: Parallel tracks for Live APMC Spot Prices (data.gov.in / Agmarknet proxy) and Econometric Price Forecasting (Seasonal Naive & Lag Ridge regression with anti-leakage chronological split).
5. **[Weather System Pipeline](file:///c:/Users/DELL/Krishi-Mitra-1/docs/flowcharts/weather_pipeline.mmd)**: OpenWeatherMap proxy, coordinate/city resolution, 5-minute dual-layer caching, 503 fast-fail guard, and deterministic spray window / fungal risk rules.
6. **[Farmer Scheme Discovery](file:///c:/Users/DELL/Krishi-Mitra-1/docs/flowcharts/farmer_scheme_discovery.mmd)**: Client-side scheme filtering, eligibility matching, and secure outbound handoff to certified government domains.
7. **[Marketplace & Order Flow](file:///c:/Users/DELL/Krishi-Mitra-1/docs/flowcharts/farmer_marketplace_orders.mmd)**: Explicit separation of current verified mandi dashboard from proposed future transactional marketplace (listings, bidding, escrow, logistics).
8. **[Complete Farmer User Journey](file:///c:/Users/DELL/Krishi-Mitra-1/docs/flowcharts/complete_farmer_user_journey.mmd)**: Mermaid user journey tracking farmer actions across onboarding, disease scanning, crop planning, mandi intelligence, weather alerts, and schemes.

---

## ⚡ Quick Verification Commands

### 1. Run Automated Backend Pytest Suite (41 Tests)
```bash
python -m pytest krishi-backend/tests/test_backend.py -v
```

### 2. Start Backend ASGI Server
```bash
cd krishi-backend
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```
Interactive Swagger documentation available at: `http://127.0.0.1:8000/docs`

### 3. Start Frontend SPA Dev Server
```bash
npm run dev
```
Application accessible at: `http://localhost:5173/`

### 4. Retrain Tabular Crop Recommendation Model
```bash
python krishi-backend/crop_recommendation/train.py
```
Outputs model weights, scaler, encoder, and `metadata.json` with 99.55% test accuracy.
