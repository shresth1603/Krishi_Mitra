# 🤖 Krishi Mitra — AI & Agent Architecture Specification

> **Document Status**: [VERIFIED IMPLEMENTED] for Gemini GenAI Agent; [PLANNED] for Multi-Agent Orchestration  
> **LLM SDK**: `google-genai` (v1.0.0+)  
> **Key Finding**: Gemini does **not** perform plant disease classification (handled by MobileNetV2 CNN) nor price forecasting (handled by Ridge / Seasonal Naive). Gemini generates agronomic recovery text and powers the conversational advisory assistant.

---

## 1. System Taxonomy: Five AI & Algorithmic Paradigms in Krishi Mitra

To prevent confusion between statistical machine learning, generative language models, and deterministic code, Krishi Mitra explicitly delineates five computational paradigms:

| Paradigm | Method / Technology | Input | Output | Example in Krishi Mitra |
| :--- | :--- | :--- | :--- | :--- |
| **1. Computer Vision ML Prediction** | MobileNetV2 CNN (Transfer Learning on 38 classes) | Normalized Leaf Pixel Tensor `(1, 224, 224, 3)` | Softmax probability distribution across 38 classes | Predicting `Tomato___Early_blight` (91.5% confidence) |
| **2. Tabular ML Prediction** | Random Forest Classifier (200 decision trees) | 7 Numerical Features: `N`, `P`, `K`, `temp`, `humidity`, `pH`, `rain` | Discrete Crop Label + Class Probabilities | Recommending `Rice` (94.2% confidence) |
| **3. Econometric Time-Series Prediction** | Lag-Feature Ridge Regression & Seasonal Naive | Chronological Daily Modal Prices (₹/q) | Projected Future Modal Price (1–14 days) | Forecasting Tomato Modal Price: ₹1,240/q |
| **4. Deterministic Rule-Based Advisory** | Hardcoded Boolean Logic in JavaScript/Python | Real-time weather variables (`windSpeed`, `humidity`) | Operational Directive | If `wind > 20 km/h`, display: *"Do Not Spray Today"* |
| **5. Generative LLM Advisory** | Google Gemini (google-genai SDK, temperature: 0.3) | Formatted Prompt with Crop, Pathogen, Season, Land Size | Natural Language 48-Hour Recovery Plan | Recommending generic Mancozeb dosage & economic loss estimate |

---

## 2. Gemini GenAI Agent Implementation

### A. Core Responsibilities
1. **Advisory Generation (`POST /analyze` & `POST /advisory`)**:
   - Takes the output of the MobileNetV2 classifier (`crop`, `disease`, `confidence`) and synthesizes an actionable, empathetic 48-hour recovery plan for Indian farmers.
   - Enforces generic chemical names (e.g. *Mancozeb 75% WP*, *Copper Hydroxide*) rather than expensive proprietary brands.
   - Computes localized rupee loss estimates based on farmer land size (e.g. ₹3,000 – ₹8,000 for 2 acres).
2. **Conversational Agricultural Assistant (`POST /api/chat`)**:
   - Answers free-form farmer questions regarding fertilizer dosages, pest identification, sowing windows, and weather adaptations.
   - Injects contextual telemetry: farmer's state, current weather, and active crops.
   - Responds in the farmer's selected language (Hindi, Malayalam, Bengali, Telugu, Marathi, English).

### B. Prompt Engineering & Structure
The backend utilizes strictly governed system prompts ([main.py:L254-L364](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L254-L364)):

#### Detailed Advisory Prompt Contract:
```text
You are an expert agricultural advisor for Indian farmers. Respond ONLY in the exact format below.
A farmer's crop photo was analyzed by an AI model:
- Crop: {crop}
- Disease Detected: {disease}
- Model Confidence: {confidence}
- Farmer's Region: {region}
- Current Season: {season}
- Land Size: {land_acres} acres

DISEASE_SUMMARY: [2-3 sentences explaining what this disease is and how it spreads]
SEVERITY: [{severity}]
VISIBLE_SYMPTOMS:
- [symptom 1]
- [symptom 2]
IMMEDIATE_ACTION_48H:
- [specific action 1]
- [specific action 2]
CHEMICAL_TREATMENT: [generic chemical name, exact dosage per litre water]
ORGANIC_ALTERNATIVE: [low-cost organic/home remedy]
PREVENTION_NEXT_SEASON:
- [prevention tip 1]
ECONOMIC_IMPACT: [Estimate rupee loss if untreated for {land_acres} acres]
CALL_OFFICER_IF: [specific condition to escalate to local KVK]
FARMER_TIP: [practical tip in simple language]
```

### C. Safety Guardrails & Quota Defense
1. **Sliding-Window Rate Limiter**:
   - `RATE_LIMIT_RPM = 12`: Allows maximum 12 requests per minute.
   - `RATE_LIMIT_RPD = 18`: Caps daily usage at 18 requests per day (reserving a 2-request buffer below free-tier 20 RPD limits).
2. **Tri-Tier Model Fallback Chain**:
   - Primary: `gemini-2.5-flash`
   - Fallback 1: `gemini-2.5-flash-lite`
   - Fallback 2: `gemini-flash-latest`
3. **Structured Static Rule Base**:
   - If all Gemini models return 429 quota errors or the internet is down, `_fallback_advisory()` activates.
   - Returns curated, agronomist-verified recipes for Early Blight, Late Blight, Bacterial Spot, Powdery Mildew, Leaf Scorch, and Mosaic Virus.
   - Guarantees that the farmer **never** receives a blank screen or unhandled 500 error.

---

## 3. Multi-Agent System: Current State vs. Proposed Architecture

### Current State: `[Single-Agent Hybrid Architecture]`
Currently, Krishi Mitra operates a **monolithic intelligent agent** embedded inside FastAPI endpoints:
- A single controller orchestrates input validation, calls specialized ML tools (MobileNetV2 or Random Forest), injects weather context, and queries Gemini.
- No multi-agent communication frameworks (e.g. LangGraph, CrewAI, AutoGen) are currently used.

### Proposed Architecture: `[Future Multi-Agent Orchestration Framework]`

```
                          ┌────────────────────────┐
                          │    SUPERVISOR AGENT    │
                          │   (Intent Router &     │
                          │    State Coordinator)  │
                          └───────────┬────────────┘
                                      │
       ┌──────────────────┬───────────┴───────────┬──────────────────┐
       ▼                  ▼                       ▼                  ▼
┌──────────────┐   ┌──────────────┐        ┌──────────────┐   ┌──────────────┐
│  DIAGNOSTIC  │   │  AGRONOMIC   │        │   MARKET     │   │  GOVERNMENT  │
│    AGENT     │   │PLANNER AGENT │        │ANALYST AGENT │   │ SCHEME AGENT │
├──────────────┤   ├──────────────┤        ├──────────────┤   ├──────────────┤
│- MobileNetV2 │   │- Random Forest│       │- AGMARKNET   │   │- schemes.js  │
│- Leaf Vision │   │  Crop Rec    │        │  Spot Ingest │   │- State DBT   │
│- Severity    │   │- NPK Balance │        │- Lag-Ridge   │   │  Eligibility │
│  Calculator  │   │- Weather     │        │  Forecaster  │   │- Document    │
│- 48h Recovery│   │  Spraying    │        │- Mandi Spreads│  │  Checklists  │
└──────────────┘   └──────────────┘        └──────────────┘   └──────────────┘
```

#### Specialized Agent Roles in Proposed Architecture:
1. **Supervisor Agent**:
   - Analyzes incoming voice or text query intent.
   - Routes multimodal requests (image -> Diagnostic Agent; soil numbers -> Agronomic Planner Agent; price inquiries -> Market Analyst Agent).
   - Synthesizes findings into a unified farmer report.
2. **Diagnostic Agent**:
   - Specializes exclusively in plant pathology, leaf symptom extraction, and biological containment protocols.
3. **Agronomic Planner Agent**:
   - Manages soil nutrient balancing, micro-climate adaptation, and crop rotation schedules.
4. **Market Analyst Agent**:
   - Monitors live price spreads across APMC mandis, identifies optimal selling days using time-series models, and calculates transport arbitrage.
5. **Government Scheme Agent**:
   - Automatically cross-references diagnosed crop losses or recommended crops against active Central and State subsidy programs (e.g. triggering PMFBY claim guidance upon diagnosing 70% Late Blight loss).
