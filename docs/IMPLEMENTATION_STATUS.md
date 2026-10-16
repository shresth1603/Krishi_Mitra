# 📊 Krishi Mitra — System Implementation Status Matrix

> **Audit Standard**: Evidence-Based Verification against Source Files, Unit Tests, and Running Endpoints  
> **Status Taxonomy**:  
> - `[VERIFIED IMPLEMENTED]`: Code exists, model/data is loaded, API route is live and verified by automated tests.  
> - `[PARTIALLY IMPLEMENTED]`: Feature exists with functioning frontend/backend, but relies on mock fallback under specific conditions.  
> - `[PLANNED]`: Architectural concept or roadmap feature discussed in documentation, but no functional code exists.  
> - `[MOCKED]`: Simulated on the client or server without genuine external service or model backing.  
> - `[NOT FOUND]`: Claimed or suggested capability with no corresponding file, route, or logic.  
> - `[UNVERIFIED]`: Code is present, but external credentials or live provider environments were unreachable during testing.

---

## 1. Comprehensive Feature Implementation Matrix

| Component / Feature | Category | Declared Status | Verification Evidence & File Reference | Operational Notes & Limitations |
| :--- | :--- | :--- | :--- | :--- |
| **Plant Disease CNN Inference** | ML Model 1 | `[VERIFIED IMPLEMENTED]` | [main.py:L734-L819](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L734-L819), [krishi_model.h5](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/model/krishi_model.h5) (10.1 MB) | MobileNetV2 architecture trained on 38 PlantVillage classes. Image input verified by Pillow (rejects corrupt bytes). Real model inference active when `USE_MOCK_MODEL=false`. |
| **Plant Disease Mock Fallback** | ML Model 1 | `[VERIFIED IMPLEMENTED]` | [main.py:L224-L230](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L224-L230), [test_backend.py:L362-L373](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/tests/test_backend.py#L362-L373) | When `USE_MOCK_MODEL=true`, explicitly flags `"is_mock": true` and `"model_version": "MOCK/DEMO (random)"`. Does NOT pass random outputs off as real ML. |
| **48-Hour Recovery Plan Generator** | GenAI / Rule | `[VERIFIED IMPLEMENTED]` | [main.py:L445-L479](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L445-L479), [DiseaseResultCard.jsx](file:///c:/Users/DELL/Krishi-Mitra-1/src/components/DiseaseResultCard.jsx) | Gemini GenAI generates structured recovery advice. If API key is absent or quota hits 429, seamlessly falls back to structured static rule base (Mancozeb, Copper, Neem). |
| **Crop Recommendation Model** | ML Model 2 | `[VERIFIED IMPLEMENTED]` | [train.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/crop_recommendation/train.py), [inference.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/crop_recommendation/inference.py), [metadata.json](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/models/crop_recommendation/metadata.json) | Random Forest (200 trees) trained on 2,200 samples across 22 crops. Verified test accuracy: 99.55%, 5-fold CV: 99.59%. Standalone endpoint `POST /api/crop-recommendation`. |
| **Crop Recommendation UI** | Frontend SPA | `[VERIFIED IMPLEMENTED]` | [CropRecommendationPage.jsx](file:///c:/Users/DELL/Krishi-Mitra-1/src/pages/CropRecommendationPage.jsx) | Interactive form with presets (Rice, Wheat, Cotton, Apple), parameter validation, top-5 alternative rankings, and agronomic disclaimers. |
| **APMC Mandi Spot Prices (Live API)** | External API | `[PARTIALLY IMPLEMENTED]` | [main.py:L1040-L1110](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L1040-L1110) | Connects to official `data.gov.in` AGMARKNET API when `AGMARKNET_KEY` is provided. If key is missing or API errors, falls back to state price estimate. |
| **APMC Mandi Mock Fallback** | Backend Data | `[VERIFIED IMPLEMENTED]` | [main.py:L1114-L1150](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L1114-L1150) | Returns indicative state prices with explicit label: `"Estimated (Agmarknet key not configured)"`. Never claims fallback data is live government data. |
| **Mandi Price Forecasting Engine** | ML Model 3 | `[VERIFIED IMPLEMENTED]` | [forecasting.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/mandi_forecast/forecasting.py), [test_backend.py:L433-L468](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/tests/test_backend.py#L433-L468) | Strict chronological split (80/20 past/future), Seasonal Naive baseline, Lag-Feature Ridge regression (lags 1,2,3,7, rolling 7-day mean). All unit tests pass. |
| **Mandi Price Historical Dataset** | Dataset Archive | `[PLANNED]` | [service.py:L44-L62](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/mandi_forecast/service.py#L44-L62) | Live API only provides daily spot prices. Historical multi-month CSVs must be downloaded from data.gov.in into `datasets/mandi_historical/`. Pipeline reports `DATA_UNAVAILABLE` rather than fabricating numbers. |
| **Agro-Weather Proxy & Cache** | External API | `[VERIFIED IMPLEMENTED]` | [main.py:L906-L1003](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L906-L1003) | Proxies OpenWeatherMap by lat/lon or city. Implements server-side 5-minute cache (`_cache`), converts wind speed to km/h, and builds 18h hourly and 5-day daily forecasts. |
| **Weather Agricultural Advisory Rules** | Rule-Based | `[VERIFIED IMPLEMENTED]` | [weatherService.js:L128-L166](file:///c:/Users/DELL/Krishi-Mitra-1/src/services/weatherService.js#L128-L166) | Deterministic rules trigger warnings: "Do Not Spray Today" (wind > 20 km/h or rain > 40%), "High Fungal Risk" (humidity > 80%, temp 22-32°C), "Extreme Heat" (> 38°C). |
| **Farmer AI Chat Proxy** | GenAI / Agent | `[VERIFIED IMPLEMENTED]` | [main.py:L850-L903](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L850-L903), [geminiService.js](file:///c:/Users/DELL/Krishi-Mitra-1/src/services/geminiService.js) | Backend proxy keeps `GEMINI_API_KEY` server-side. Injects farmer state, weather, active crops. Enforces 12 RPM / 18 RPD sliding-window rate limit. Returns friendly KVK fallback on quota limit. |
| **Voice Input & Audio Readout** | Web Speech API | `[VERIFIED IMPLEMENTED]` | [voiceService.js](file:///c:/Users/DELL/Krishi-Mitra-1/src/services/voiceService.js), [ChatPage.jsx:L113-L120](file:///c:/Users/DELL/Krishi-Mitra-1/src/pages/ChatPage.jsx#L113-L120) | Web Speech API SpeechRecognition for Hindi, Malayalam, Bengali, Telugu, Marathi, and English. SpeechSynthesis for conversational voice playback. |
| **Government Schemes Directory** | Frontend Data | `[VERIFIED IMPLEMENTED]` | [schemes.js](file:///c:/Users/DELL/Krishi-Mitra-1/src/data/schemes.js), [SchemesPage.jsx](file:///c:/Users/DELL/Krishi-Mitra-1/src/pages/SchemesPage.jsx) | Searchable and filterable by state and category. Contains PM-KISAN, PMFBY, KCC, PKVY, MGNREGS, RKVY, and 4 state-specific schemes. Direct links to official portals. |
| **User Authentication & Session** | Auth / Storage | `[MOCKED]` | [AuthContext.jsx](file:///c:/Users/DELL/Krishi-Mitra-1/src/context/AuthContext.jsx) | Simulates login, signup, and Google OAuth using browser `localStorage`. No database, password hashing, or JWT verification exists in backend. |
| **Transactional Marketplace** | E-Commerce | `[PLANNED]` | [MARKETPLACE_AND_ORDER_FLOW.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/MARKETPLACE_AND_ORDER_FLOW.md) | Platform is an information dashboard, not an e-commerce platform. No shopping cart, product listings, payment gateway (Razorpay/UPI), or order tracking exist. |
| **Crop Yield Estimation** | ML Feature | `[PLANNED]` | [KRISHI_MITRA_ML_RESEARCH_AND_ANTIGRAVITY_PROMPT.md:L73-L78](file:///c:/Users/DELL/Krishi-Mitra-1/KRISHI_MITRA_ML_RESEARCH_AND_ANTIGRAVITY_PROMPT.md#L73-L78) | Identified as future expansion using Ministry of Agriculture district-wise production statistics. No code or dataset exists in current codebase. |
| **Multi-Agent Orchestration** | AI System | `[PLANNED]` | [AI_AGENT_ARCHITECTURE.md](file:///c:/Users/DELL/Krishi-Mitra-1/docs/AI_AGENT_ARCHITECTURE.md) | Current system uses a single prompt-based Gemini agent. No LangChain, CrewAI, AutoGen, or multi-agent supervisor is currently integrated. |

---

## 2. Test Verification Summary

Automated testing was conducted on October 2026 using Python 3.13 and Pytest 9.1.1 on Windows:

- **Command**: `python -m pytest krishi-backend/tests/test_backend.py -v`
- **Total Tests Collected**: 41
- **Total Tests Passed**: **41 (100% Pass Rate)**
- **Total Tests Failed**: 0
- **Duration**: 198.09 seconds

### Test Coverage Highlights:
1. **Health & Classes**: `/health` status and all 38 PlantVillage label mappings verified.
2. **Plant Disease Robustness**: Corrupt image byte rejection (HTTP 400), image dimension handling, mock flag reporting (`is_mock: true` when simulated).
3. **Crop Recommendation**: Valid input processing (HTTP 200), out-of-range pH/Nitrogen rejection (HTTP 422), missing field handling (HTTP 422), model info metadata contract.
4. **Mandi Prices & Forecasting**: Agmarknet mock fallback (HTTP 200), forecast horizon validation (rejects <= 0 or > 30 with HTTP 422), historical archive absence reporting (`DATA_UNAVAILABLE`), chronological split with zero future leakage, baseline seasonal naive and Ridge regression algorithmic correctness.
5. **Chat AI Proxy**: Empty question rejection (HTTP 400), valid chat question response, fallback handling on quota limits.
