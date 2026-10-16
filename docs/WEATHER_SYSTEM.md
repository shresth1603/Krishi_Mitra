# 🌤️ Krishi Mitra — Weather System Architecture & 503 Diagnosis

> **Document Status**: [VERIFIED IMPLEMENTED]  
> **Provider**: OpenWeatherMap API v2.5 (`https://api.openweathermap.org/data/2.5/`)  
> **Audited Modules**: [main.py:L906-L1003](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L906-L1003), [weatherService.js](file:///c:/Users/DELL/Krishi-Mitra-1/src/services/weatherService.js), [useWeather.js](file:///c:/Users/DELL/Krishi-Mitra-1/src/hooks/useWeather.js), [WeatherPage.jsx](file:///c:/Users/DELL/Krishi-Mitra-1/src/pages/WeatherPage.jsx)  

---

## 1. End-to-End Request Trace

```
[Farmer Selects State & District on WeatherPage.jsx]
                          │
                          ▼
[weatherService.js: getCurrentWeather(district)]
  ├── 1. Check client-side memory cache (_wCache) — 5-minute TTL
  ├── 2. Coordinate Lookup: DISTRICT_COORDS[district]
  │      ├── Hit:  build URL -> /api/weather?lat={lat}&lon={lon}
  │      └── Miss: build URL -> /api/weather?city={district}
                          │
                          ▼ (HTTP GET)
[FastAPI Backend /api/weather in main.py]
  ├── 1. Verify WEATHER_API_KEY existence in .env
  │      └── If empty -> Return HTTP 503 Service Unavailable
  ├── 2. Check server-side memory cache (_cache) — 300s TTL
  ├── 3. Asynchronously call OpenWeatherMap via httpx.AsyncClient (8.0s timeout):
  │      ├── GET /data/2.5/weather?lat=...&units=metric (Current)
  │      └── GET /data/2.5/forecast?lat=...&units=metric (5-Day / 3-Hour)
  ├── 4. Normalize JSON:
  │      ├── Convert wind speed from m/s to km/h (* 3.6)
  │      ├── Extract next 6 slots (18 hours) for Hourly Timeline
  │      └── Group by unique date to compute 5-Day Daily High/Low ranges
  ├── 5. Write to server cache (cache_set)
                          │
                          ▼ (HTTP 200 JSON Payload)
[Frontend Response Processing in WeatherPage.jsx]
  ├── 1. Render Current Conditions, Humidity, Wind, Pressure Cards
  ├── 2. Feed weather payload into getFarmingAdvisory(weather)
  │      └── Evaluates Spray, Fungal Risk, Irrigation, Heat/Cold alerts
  ├── 3. Save snapshot to LocalStorage ('km_weather') for Chatbot context
  └── 4. Render 18-Hour Timeline and 5-Day Forecast Grid
```

---

## 2. Root Cause Analysis: The Observed 503 Weather Error

During project audits and initial setup, testers observed HTTP 503 errors when loading `/weather`. A code-level investigation reveals the precise deterministic root cause:

### Source Code Evidence ([main.py:L917-L918](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L917-L918)):
```python
if not WEATHER_API_KEY:
    raise HTTPException(
        status_code=503, 
        detail="Weather service not configured. Set WEATHER_API_KEY in backend .env."
    )
```

### Deterministic Breakdown:
1. **Unconfigured Environment Variable**: If the developer copies `env.example` to `.env` but leaves `WEATHER_API_KEY=your_openweather_api_key_here` or leaves it empty, `WEATHER_API_KEY` evaluates to falsy or unauthenticated.
2. **Immediate Fast-Fail Guard**: Rather than making a failing HTTP call to OpenWeatherMap and incurring latency, the backend raises `HTTPException(503)`.
3. **Frontend Error Propagation**: `weatherService.js` reads `res.status` and throws `Error("Weather service not configured. Set WEATHER_API_KEY in backend .env.")`.
4. **UI Behavior**: `WeatherPage.jsx` catches the error and cleanly displays an honest alert box with a **"Retry"** button rather than crashing or showing fabricated weather data.
5. **Resolution**: Obtain a free OpenWeatherMap API key at [https://openweathermap.org/api](https://openweathermap.org/api), place it in `krishi-backend/.env`, and restart the ASGI server.

---

## 3. Data Schema & Normalization

| Field Name | Raw OWM Key | Normalized Backend Key | Physical Unit | Normalization Applied |
| :--- | :--- | :--- | :--- | :--- |
| **Location Name** | `name` | `location` | String | Extracted from current weather response |
| **Country Code** | `sys.country` | `country` | ISO 3166 | Default: `"IN"` |
| **Current Temp** | `main.temp` | `temperature` | °C | Rounded integer (`units=metric`) |
| **Feels Like** | `main.feels_like`| `feels_like` | °C | Rounded integer |
| **Condition** | `weather[0].main`| `condition` | String | e.g. `"Clear"`, `"Rain"`, `"Clouds"` |
| **Description** | `weather[0].description`| `description` | String | e.g. `"moderate rain"`, `"scattered clouds"` |
| **Relative Humidity**| `main.humidity` | `humidity` | % | Integer (0–100) |
| **Wind Velocity** | `wind.speed` | `windSpeed` | km/h | Converted from m/s (`round(speed * 3.6)`) |
| **Atmospheric Pressure**| `main.pressure` | `pressure` | hPa / mbar | Integer (typically 1000–1025) |
| **Visibility** | `visibility` | `visibility` | km | Converted from meters (`round(vis / 1000, 1)`) |
| **Weather Icon** | `weather[0].icon`| `icon` | String code | OWM icon identifier (e.g. `"10d"`, `"01n"`) |
| **Hourly Forecast** | `list[0:6]` | `hourlyForecast` | Array of 6 slots| 18-hour projection with `time`, `temp`, `condition`, `rain%` |
| **Daily Forecast** | `list` | `weeklyForecast` | Array of 5 days | Daily high/low temperature bounds & midday icon |

---

## 4. Agricultural Decision Engine (`getFarmingAdvisory`)

Unlike standard commercial weather apps that merely show temperatures, Krishi Mitra applies **deterministic agronomic rules** ([weatherService.js:L128-L166](file:///c:/Users/DELL/Krishi-Mitra-1/src/services/weatherService.js#L128-L166)) to translate atmospheric data into farm operations:

### Rule 1: Chemical Spray Window Guard
- **Condition**: `windSpeed > 20 km/h` OR `rainProbability > 40%`
- **Output**: 🚫 **"Do Not Spray Today"** — High wind causes chemical drift onto neighboring fields; imminent rain washes expensive fungicides off leaves before absorption.
- **Counter Condition**: `windSpeed < 12 km/h` AND `rainProbability < 20%` -> ✅ **"Safe Spray Window (6:00 AM – 9:00 AM)"**.

### Rule 2: Fungal Disease Outbreak Alert
- **Condition**: `humidity > 80%` AND `22°C < temperature < 32°C`
- **Output**: 🍄 **"High Fungal Disease Risk"** — Ideal microclimate for Rice Blast (*Magnaporthe oryzae*), Potato Late Blight (*Phytophthora infestans*), and Downy Mildew. Recommends prophylactic protective sprays.

### Rule 3: Extreme Thermal Stress Warnings
- **High Heat Condition**: `temperature > 38°C` -> 🌡️ **"Extreme Heat Alert"** — High transpiration leads to blossom drop and wilting; recommends deploying shade nets for nurseries and irrigating in evening.
- **Cold Stress Condition**: `temperature < 10°C` -> 🥶 **"Cold Stress Warning"** — High frost risk; cover nursery beds overnight.

### Rule 4: Waterlogging & Drainage Alert
- **Condition**: `condition == "Thunderstorm"` OR `rainProbability > 70%`
- **Output**: ⛈️ **"Heavy Rain — Clear Field Drainage"** — Prevents root suffocation, collar rot, and waterlogging in low-lying alluvial plots.

---

## 5. Resilient Architecture Recommendations

To transition the weather system into an enterprise-grade agricultural telemetry pipeline:
1. **Multi-Provider Fallback**: Add secondary weather providers (e.g., India Meteorological Department / IMD Mausam API, or WeatherAPI) if OpenWeatherMap exceeds free quotas.
2. **Persistent Caching**: Replace the current in-memory dictionary with a Redis cache to survive backend worker restarts.
3. **Hyperlocal Soil Sensor Integration**: Support IoT soil moisture telemetry (e.g. LoRaWAN soil moisture probes) alongside atmospheric satellite feeds.
