# 🎤 Krishi Mitra — Academic Viva, Evaluator Defense & Presentation Notes

> **Target Audience**: College Faculty, Project Evaluators, Technical Mentors, Teammates, Agritech Investors  
> **Presentation Duration**: 10–15 Minutes  
> **Key Objective**: Deliver an evidence-based, technically rigorous, and honest demonstration of Krishi Mitra.

---

## 1. The 2-Minute Elevator Pitch

> *"Respected evaluators and mentors, Indian agriculture directly employs over 45% of our nation's workforce, yet smallholder farmers lose up to 35% of their crop yield each year to plant diseases, unseasonal weather, and suboptimal crop selection.*
>
> *Existing mobile tools are fragmented: one app checks weather, another shows mandi prices, and government schemes are buried in complex bureaucratic portals. Furthermore, many student prototypes use random mock data disguised as AI.*
>
> *We built **Krishi Mitra** as an integrated agricultural decision platform. It combines a 38-class MobileNetV2 computer vision model for plant disease diagnosis, a 99.55% accurate Random Forest model for soil-climate crop suitability, real-time APMC mandi spot prices and econometric forecasting from official government catalogs, hyperlocal weather risk alerts, and a multilingual voice-enabled GenAI assistant. Every claim in our system is backed by verifiable code and 41 passing automated tests."*

---

## 2. Live Demonstration Script (Step-by-Step)

### Step 1: Real-Time Plant Disease Diagnosis (`/chat` or Leaf Scanner)
1. Navigate to `/chat` and click the **Camera / Upload** button.
2. Select a leaf photograph (e.g. Tomato leaf showing Early Blight symptoms).
3. **Show Evaluators**:
   - The UI shows real-time progress: *"Uploading image..."* -> *"Analyzing..."* -> *"Writing advisory..."*.
   - The MobileNetV2 CNN infers the class (`Tomato___Early_blight`) with confidence score.
   - The system separates model confidence from agronomic severity.
   - A structured **48-Hour Action Plan** appears with specific generic chemical dosages (Mancozeb 75% WP @ 2.5g/L) and low-cost organic alternatives (Neem oil spray).
   - Tap **"Listen"** to demonstrate vernacular text-to-speech audio readout in Hindi or Malayalam.

### Step 2: Intelligent Crop Recommendation (`/crop-recommendation`)
1. Navigate to `/crop-recommendation`.
2. Demonstrate the quick-preset buttons (e.g., tap *"Wheat / Rabi Loam"* or *"Rice / Kharif Alluvial"*).
3. Explain the 7 agronomic inputs: Nitrogen, Phosphorus, Potassium, Temperature, Humidity, Soil pH, and Rainfall.
4. Click **"Recommend Best Crop"**.
5. **Show Evaluators**:
   - Instant response from the Scikit-Learn Random Forest model (200 trees).
   - Primary recommendation with confidence percentage.
   - Top-5 alternative candidate crops with comparative suitability bars.
   - Grounded disclaimer emphasizing consultation with local Krishi Vigyan Kendra (KVK).

### Step 3: APMC Mandi Spot Prices & Econometric Forecasting (`/market-prices`)
1. Navigate to `/market-prices`.
2. Select State (e.g., `Maharashtra` or `Uttar Pradesh`) and Commodity (e.g., `Tomato` or `Wheat`).
3. **Tab 1 — Spot Market Prices**:
   - Displays real-time wholesale prices across APMC mandis (Minimum, Maximum, Modal price per Quintal) with spread insights.
4. **Tab 2 — Price Forecasting**:
   - Switch to the forecasting tab.
   - Explain the anti-fabrication policy: when historical depth is below 30 days, the engine honestly displays `DATA_UNAVAILABLE` and details ingestion requirements, proving the platform never fabricates financial predictions.

### Step 4: Hyperlocal Agro-Weather & Spraying Alerts (`/weather`)
1. Navigate to `/weather`.
2. Select State and District (e.g., `Kerala -> Thiruvananthapuram` or `Uttar Pradesh -> Lucknow`).
3. **Show Evaluators**:
   - Live temperature, feels-like, humidity, wind velocity, and pressure.
   - **Farming Decision Alerts**: Point out the deterministic agronomic rules:
     - *"Do Not Spray Today"* if wind > 20 km/h or rain > 40%.
     - *"High Fungal Risk"* if humidity > 80% and temp 22–32°C.
   - 18-hour timeline and 5-day extended weather trends.

### Step 5: Government Scheme Discovery (`/schemes`)
1. Navigate to `/schemes`.
2. Filter by category (e.g., *"Income Support"* or *"Credit & Loans"*).
3. Search for *"organic"* or *"PM-KISAN"*.
4. **Show Evaluators**:
   - Clear breakdown of rupee benefits, eligibility rules, and required document checklists.
   - Click **"Apply on Official Portal"**: demonstrates how Krishi Mitra redirects to `pmkisan.gov.in`, preserving sensitive personal data (Aadhaar/Bank details) outside the platform.

---

## 3. Evaluator Defense & Common Viva Questions

### Q1: "Is your plant disease model trained on real Indian field images?"
**Defense**:
> *"No, and we explicitly document this limitation in our audit. The model is trained on the standard 38-class PlantVillage dataset, which was photographed in controlled laboratory environments on uniform backgrounds. While plant pathogen biology (such as Early Blight or Powdery Mildew) is universal, real Indian field images contain complex weed backgrounds and sun glare.*
> *In our documentation (`DATASET_AUDIT.md`), we propose expanding the model using the official ICAR Rice and Maize field dataset from AIKosh (926 MB), which contains authentic Indian open-field conditions."*

### Q2: "Why use a Random Forest for crop recommendation instead of a Deep Neural Network?"
**Defense**:
> *"For tabular agricultural data with continuous features (NPK, pH, temperature, rainfall), tree-based ensemble methods like Random Forest consistently outperform deep neural networks in sample efficiency, interpretability, and inference latency.*
> *Our Random Forest achieved **99.55% test accuracy** and **99.59% 5-fold cross-validation**, while computing Gini feature importance (confirming rainfall and humidity account for over 43% of crop suitability variance). It executes in under 15 milliseconds without requiring GPU hardware on the server."*

### Q3: "Does your application allow farmers to buy and sell produce online?"
**Defense**:
> *"No. Krishi Mitra is currently an informational market intelligence dashboard, not a transactional marketplace. We display daily wholesale mandi prices from the official AGMARKNET portal to give farmers price transparency during offline negotiations.*
> *In `MARKETPLACE_AND_ORDER_FLOW.md`, we explain the technical and regulatory difference between B2B, F2B, and F2C models, and provide a 4-phase roadmap for adding listings, escrow payments, and logistics in future iterations."*

### Q4: "How does your backend prevent API key exposure and quota exhaustion?"
**Defense**:
> *"First, all third-party API keys (`GEMINI_API_KEY`, `WEATHER_API_KEY`, `AGMARKNET_KEY`) are encapsulated server-side inside FastAPI; the browser bundle contains zero secret tokens.*
> *Second, we engineered an in-memory sliding-window rate limiter (12 RPM, 18 RPD) to protect free-tier Gemini limits.*
> *Third, we implemented a tri-tier model fallback chain (`gemini-2.5-flash` -> `gemini-2.5-flash-lite` -> `gemini-flash-latest`), backed by a curated static rule base that guarantees farmers always receive expert treatment guidance even during complete network or quota failure."*

### Q5: "What caused the 503 error on the weather page during testing?"
**Defense**:
> *"Our code audit revealed a deliberate fast-fail guard in `main.py`: if `WEATHER_API_KEY` is unset or empty in `.env`, the backend raises an `HTTPException(503)` rather than making a failing external call. The frontend catches this and displays an honest retry banner. Once a valid OpenWeatherMap key is configured, the endpoint returns HTTP 200 with 5-minute memory caching."*

### Q6: "How do you protect sensitive farmer data like Aadhaar or land records?"
**Defense**:
> *"In compliance with the Digital Personal Data Protection Act (DPDP Act, 2023), Krishi Mitra has a zero-collection policy for sensitive government identifiers. We display eligibility checklists and then hand the user off directly to official government portals (`.gov.in` / `.nic.in`) for authentication via Aadhaar OTP or DigiLocker."*

---

## 4. Key Metrics Summary for Slides & Report

- **Automated Test Suite**: 41/41 passing unit/integration tests (`pytest`)
- **Plant Disease Classes**: 38 classes across 14 crops (26 diseased, 12 healthy)
- **Crop Recommendation Accuracy**: 99.55% Test Accuracy, 99.59% 5-Fold Cross-Validation
- **Supported Crops**: 22 Indian agricultural crops
- **Mandi Market Coverage**: 2,700+ registered APMC mandis via AGMARKNET
- **Geographic Telemetry**: 48 pre-mapped Indian districts + universal city geocoding
- **Language Localization**: 6 Indian languages supported (Hindi, Malayalam, Bengali, Telugu, Marathi, English)
