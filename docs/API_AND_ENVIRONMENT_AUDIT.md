# 🔐 Krishi Mitra — API Integrations & Environment Variables Audit

> **Security Standard**: ZERO credential exposure. Secret keys, bearer tokens, and credentials are never printed or revealed.  
> **Audited Date**: October 2026  
> **Backend Config**: `krishi-backend/env.example` & `krishi-backend/.env`  
> **Frontend Config**: `import.meta.env.VITE_API_URL`  

---

## 1. Environment Variable Inventory & Security Matrix

| Environment Variable | Service / Purpose | Defined In | Consumed In | Server / Client Boundary | Configured Status | Security & Secret Handling |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `GEMINI_API_KEY` | Google GenAI API (Generative Advisories & Chatbot) | `krishi-backend/.env` | [main.py:L74](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L74) | **Server-Side Only** (Never exposed to browser) | Configured in local environment | Read via `os.getenv()`. Guarded by sliding-window rate limiter. Secret value is masked. |
| `WEATHER_API_KEY` | OpenWeatherMap (Hyperlocal Conditions & 5-Day Forecast) | `krishi-backend/.env` | [main.py:L75](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L75) | **Server-Side Only** (Never exposed to browser) | Configured in local environment | Proxied via `/api/weather`. Cached for 5 minutes. Secret value is masked. |
| `AGMARKNET_KEY` | data.gov.in / AGMARKNET API (Daily APMC Spot Prices) | `krishi-backend/.env` | [main.py:L76](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L76) | **Server-Side Only** (Never exposed to browser) | Optional (Fallback active if unset) | Proxied via `/api/mandi`. If absent, gracefully falls back to state-level indicative data. |
| `MODEL_PATH` | File path to pre-trained Keras HDF5 weights | `krishi-backend/.env` | [main.py:L77](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L77) | Server-Side Only | Default: `./model/krishi_model.h5` | Non-secret path string. Validated on startup via `Path(MODEL_PATH).exists()`. |
| `USE_MOCK_MODEL` | Toggle for simulated random predictions in dev mode | `krishi-backend/.env` | [main.py:L78](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L78) | Server-Side Only | Default: `false` | When `true`, explicitly sets `is_mock: true` in response. |
| `PORT` | ASGI Uvicorn HTTP server listening port | `krishi-backend/.env` | [main.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py) | Server-Side Only | Default: `8000` | Non-secret integer. |
| `ALLOWED_ORIGINS` | CORS allowed origin origins list | `krishi-backend/.env` | [main.py:L79-L82](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L79-L82) | Server-Side Only | Default: `http://localhost:5173,http://localhost:3000` | Comma-delimited domain whitelist. |
| `VITE_API_URL` | Frontend pointer to backend ASGI server | Frontend build environment | `src/services/*.js` | **Client-Side** (Bundled in SPA) | Default fallback: `http://localhost:8000` | Non-secret URL string pointing to backend API. |

---

## 2. In-Depth Service Integration Audits

### A. Google Gemini GenAI Integration
- **SDK**: `google-genai` (Official Google GenAI SDK v1.0.0+, replacing deprecated `google-generativeai`).
- **Target Tasks in Krishi Mitra**:
  1. Detailed 48-hour chemical and organic recovery plans for diagnosed leaf diseases (`POST /analyze` and `POST /advisory`).
  2. Short conversational WhatsApp formatted advice.
  3. Interactive agricultural question-answering proxy (`POST /api/chat`).
- **Model Fallback Chain**:
  - Primary: `gemini-2.5-flash` (temperature: 0.3, max_tokens: 1200)
  - Secondary: `gemini-2.5-flash-lite`
  - Tertiary: `gemini-flash-latest`
  - *Note*: `gemini-2.0-flash` is intentionally excluded due to historical quota exhaustion on free-tier projects.
- **Rate Limiting & Safety Guardrails**:
  - Internal sliding-window rate limiter: **12 Requests per Minute (RPM)**.
  - Daily quota ceiling: **18 Requests per Day (RPD)** (preserves safety buffer under free-tier limits).
  - Quota errors (HTTP 429 / `resource_exhausted`) immediately route to the structured static rule base (curated Mancozeb, Copper Hydroxide, and Neem oil recipes) without throwing unhandled 500 exceptions.
- **Documentation**: [https://ai.google.dev/docs](https://ai.google.dev/docs)

### B. OpenWeatherMap Weather Integration
- **Endpoint**: `https://api.openweathermap.org/data/2.5/`
  - Current Weather: `/weather?lat={lat}&lon={lon}&appid={KEY}&units=metric`
  - 5-Day Forecast: `/forecast?lat={lat}&lon={lon}&appid={KEY}&units=metric`
- **Frontend / Backend Connection**:
  - The browser **never** calls OpenWeatherMap directly.
  - The frontend calls `GET /api/weather?lat=...&lon=...` or `GET /api/weather?city=...`.
  - The backend proxies the request, injects `WEATHER_API_KEY`, normalizes units to metric (°C, km/h), builds 18h hourly and 5-day daily groupings, and caches the result for 300 seconds.
- **Error Handling & 503 Diagnosis**:
  - If `WEATHER_API_KEY` is not present in `.env`, the endpoint returns HTTP 503 with: `"Weather service not configured. Set WEATHER_API_KEY in backend .env."`
  - Timeout is set to 8.0s (`httpx.TimeoutException` returns HTTP 504).
- **Documentation**: [https://openweathermap.org/api](https://openweathermap.org/api)

### C. Open Government Data (data.gov.in / AGMARKNET)
- **Endpoint**: `https://api.data.gov.in/resource/9ef84268-d588-465a-a308-a864a43d0070`
- **Parameters**: `api-key={AGMARKNET_KEY}&format=json&limit={limit}&filters[State]={state}&filters[Commodity]={commodity}`
- **Operational Reality**:
  - When `AGMARKNET_KEY` is valid, returns authentic live daily arrivals from APMC mandis.
  - When unset, times out, or returns 0 records, the backend activates a **state-specific mock fallback** with 10 state pricing profiles.
  - Fallback responses explicitly identify their provenance as `"Estimated (Agmarknet key not configured)"` to maintain complete data transparency.
- **Documentation**: [https://www.data.gov.in/](https://www.data.gov.in/)

---

## 3. Security Audit & Repository Hygiene

1. **Git Tracking Verification**:
   - Inspection of `.gitignore` verifies that `.env`, `.env.local`, `*.h5`, `*.tflite`, and `*.joblib` are explicitly ignored.
   - A search of git commit messages and diffs confirms that **no production API keys or credentials were committed** to the public repository.
2. **Server-Side API Key Encapsulation**:
   - `GEMINI_API_KEY`, `WEATHER_API_KEY`, and `AGMARKNET_KEY` are read exclusively by the Python ASGI backend process.
   - The React frontend bundle contains zero secret tokens. The only frontend environment variable is `VITE_API_URL` (pointing to the backend origin).
3. **Key Rotation & Best Practices**:
   - In the event of team handoff or public demonstration, users should generate independent keys from:
     - Google AI Studio: [https://aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)
     - OpenWeatherMap: [https://home.openweathermap.org/api_keys](https://home.openweathermap.org/api_keys)
     - Data.gov.in: [https://data.gov.in/](https://data.gov.in/)
   - Never share `.env` files via email or messaging platforms; distribute via encrypted secrets managers or copy from `env.example`.
