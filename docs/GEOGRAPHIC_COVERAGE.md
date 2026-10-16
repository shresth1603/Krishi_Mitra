# 🗺️ Krishi Mitra — Geographic Coverage & Agricultural Scope Audit

> **Audit Standard**: Evidence-Based Verification of Actual Data Coverage vs. Interface Selectors  
> **Audited Date**: October 2026  
> **Key Finding**: While the frontend interface provides selectable dropdowns for all Indian states and over 200 districts, actual underlying data coverage varies significantly across functional modules.

---

## 1. Geographic Coverage Taxonomy

To ensure absolute academic and technical honesty, every region and district is classified under one of three verified statuses:
1. **Verified Data Coverage**: Genuine dataset records, pre-trained model instances, or verified live API responses exist specifically for this location.
2. **API-Selectable but Unverified Coverage**: The location appears in a dropdown selector or maps to an external API query, but data availability depends on dynamic third-party response conditions or falls back to synthetic estimates.
3. **No Evidence of Coverage**: No local data, coordinate mapping, or model validation exists for this region in the current codebase.

---

## 2. Module-by-Module Regional Audit

### A. Plant Disease Image Collection Locations
- **Status**: `[No Evidence of Indian Field Coverage]` (Benchmark-only)
- **Evidence**: Model weights (`krishi_model.h5`) are derived from the international PlantVillage benchmark.
- **Collection Granularity**: Leaves were gathered from university research farms and commercial greenhouses in the United States (Pennsylvania State University) and Switzerland (EPFL).
- **Finding**: **0 images** originated from Indian open fields or Uttar Pradesh. The model classifies morphological pathogen symptoms that are biologically universal (e.g., *Alternaria solani* early blight on tomato behaves identically across geographical boundaries), but field-level background noise in Indian soil/canopies is unrepresented.

### B. Crop Recommendation Tabular Coverage
- **Status**: `[Generalized Agro-Climatic Coverage — Unverified at District Level]`
- **Evidence**: `krishi-backend/datasets/crop_recommendation/Crop_recommendation.csv` (2,200 rows).
- **Collection Granularity**: The dataset provides generic tropical and subtropical agronomic parameters (temperature, humidity, rainfall, pH, NPK).
- **Finding**: The dataset does **not** contain a "State" or "District" column. It cannot be claimed that the model was trained on or regionally validated for Uttar Pradesh, Jalaun, or Orai. It predicts crop suitability based strictly on soil/climate physics.

### C. APMC Mandi Spot Prices & Forecasting
- **Status**: `[Verified Dynamic National Coverage with Specific Fallbacks]`
- **Evidence**: [main.py:L1040-L1150](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L1040-L1150), [MarketPricePage.jsx](file:///c:/Users/DELL/Krishi-Mitra-1/src/pages/MarketPricePage.jsx).
- **Live AGMARKNET API**: When configured with `AGMARKNET_KEY`, data.gov.in queries cover **all 28 Indian States and UTs** across 2,700+ registered APMC mandis.
- **State Mock Fallback Matrix**: If the AGMARKNET key is omitted or the API fails, hardcoded state-level indicative ranges exist for **10 specific states**:
  1. Maharashtra (Tomato, Onion, Wheat, Potato)
  2. Punjab (Wheat, Rice, Maize, Potato)
  3. **Uttar Pradesh** (Potato: ₹1200-1600, Wheat: ₹2000-2300, Sugarcane: ₹350-420, Onion: ₹600-1000)
  4. Karnataka (Tomato, Onion, Groundnut, Coconut)
  5. Kerala (Coconut, Banana, Rice, Tomato)
  6. West Bengal (Rice, Potato, Banana, Tomato)
  7. Tamil Nadu (Rice, Tomato, Onion, Banana)
  8. Gujarat (Groundnut, Cotton, Wheat, Tomato)
  9. Rajasthan (Wheat, Onion, Tomato, Maize)
  10. Madhya Pradesh (Soyabean, Wheat, Maize, Tomato)
- **District Granularity**: Fallback records use synthetic mandi names (e.g. *"Uttar Pradesh Main Mandi"*); live API returns authentic APMC mandis (e.g., *Orai Mandi*, *Kanpur Mandi*, *Lucknow Mandi*).

### D. Weather System Location Resolution & Coordinates
- **Status**: `[Verified Coordinate Mapping for 48 Major Locations; Universal Query Fallback]`
- **Evidence**: `src/services/weatherService.js` (`DISTRICT_COORDS` and `INDIA_DISTRICTS`).
- **Hardcoded Coordinates**: Exactly **48 major cities/districts** have pre-configured GPS coordinates `[latitude, longitude]`.
  - **Uttar Pradesh Hardcoded Coordinates**:
    - Lucknow: `[26.8467, 80.9462]`
    - Agra: `[27.1767, 78.0081]`
    - Kanpur: `[26.4499, 80.3319]`
    - Varanasi: `[25.3176, 82.9739]`
    - Allahabad (Prayagraj): `[25.4358, 81.8463]`
    - Meerut: `[28.9845, 77.7064]`
    - Ghaziabad: `[28.6692, 77.4538]`
    - Gorakhpur: `[26.7606, 83.3732]`
  - **Other States**: Kerala (all 14 districts), Maharashtra (12 districts), Punjab (5 districts), and major metros/state capitals.
- **Universal Query Fallback**: If a district is not in `DISTRICT_COORDS` (such as Orai/Jalaun), the frontend builds a query using the city name string: `GET /api/weather?city=Orai`. OpenWeatherMap geocodes the string on the fly.

---

## 3. Verified Geographic Coverage Matrix

| Region / District | Plant Disease Model | Crop Recommendation | Mandi Spot Prices | Weather System | Overall Regional Coverage Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Uttar Pradesh — Lucknow** | Universal Pathogens | Agronomic Physics | Live / State Fallback | Pre-mapped `[26.84, 80.94]` | **Verified Data Coverage** |
| **Uttar Pradesh — Kanpur** | Universal Pathogens | Agronomic Physics | Live / State Fallback | Pre-mapped `[26.44, 80.33]` | **Verified Data Coverage** |
| **Uttar Pradesh — Agra** | Universal Pathogens | Agronomic Physics | Live / State Fallback | Pre-mapped `[27.17, 78.00]` | **Verified Data Coverage** |
| **Uttar Pradesh — Varanasi** | Universal Pathogens | Agronomic Physics | Live / State Fallback | Pre-mapped `[25.31, 82.97]` | **Verified Data Coverage** |
| **Uttar Pradesh — Orai (Jalaun)** | Universal Pathogens | Agronomic Physics | Live AGMARKNET (if key set) | City name string query | **API-Selectable but Unverified Coverage** |
| **Uttar Pradesh — Jhansi / Bundelkhand** | Universal Pathogens | Agronomic Physics | Live AGMARKNET (if key set) | City name string query | **API-Selectable but Unverified Coverage** |
| **Kerala — All 14 Districts** | Universal Pathogens | Agronomic Physics | Live / State Fallback | Pre-mapped GPS Coordinates | **Verified Data Coverage** |
| **Maharashtra — Pune, Mumbai, Nagpur**| Universal Pathogens | Agronomic Physics | Live / State Fallback | Pre-mapped GPS Coordinates | **Verified Data Coverage** |
| **Punjab — Ludhiana, Amritsar** | Universal Pathogens | Agronomic Physics | Live / State Fallback | Pre-mapped GPS Coordinates | **Verified Data Coverage** |
| **North-East / Hill States (e.g. Sikkim)**| Universal Pathogens | Agronomic Physics | Live AGMARKNET (if key set) | Capital GPS mapped | **API-Selectable but Unverified Coverage** |

---

## 4. Uttar Pradesh & Bundelkhand (Orai/Jalaun) Pilot Deployment Strategy

While the existing codebase provides general support for Uttar Pradesh, deploying Krishi Mitra for field trials in **Bundelkhand (specifically Jalaun / Orai)** is strongly recommended for future development.

### Agronomic Context of Jalaun & Bundelkhand:
1. **Soil Profile**: Dominantly *Bundelkhand Black Soil* (Kabar/Mar) and *Red/Yellow Alluvial Soil* (Rakar/Parua), with high clay content, low organic carbon, and distinct water retention characteristics.
2. **Key Cash & Food Crops**:
   - Pulses: Chickpea (*Chana*), Pigeonpea (*Arhar*), Lentil (*Masoor*), Blackgram (*Urad*).
   - Oilseeds: Mustard (*Sarson*), Sesame (*Til*).
   - Cereals: Wheat (*Gehun*), Barley (*Jau*).
3. **Primary Agricultural Vulnerabilities**: Recurrent semi-arid drought, frost risk during winter rabi season, and acute market distance to major state consumption hubs.

### Practical Roadmap for a Bundelkhand Pilot:
- **Phase 1 (Coordinate & Mandi Grounding)**:
  - Add Orai (`[25.9898, 79.4526]`) and Jalaun (`[26.1457, 79.3364]`) directly into `DISTRICT_COORDS` in `weatherService.js`.
  - Ingest 90 days of historical arrival data from Orai APMC Mandi to activate the local time-series forecasting model.
- **Phase 2 (Pulse & Oilseed Disease Expansion)**:
  - Complement the current PlantVillage model with pulse disease datasets (Chickpea Wilt, Pod Borer, Yellow Mosaic Virus on Urad).
- **Phase 3 (Localized KVK Integration)**:
  - Connect with **KVK Jalaun (ICAR-IISR, Bharuwa Sumerpur / Orai)** for localized agronomic advisory validation.
