# 🎓 KRISHI MITRA (कृषि मित्र) — PROJECT SUBMISSION & EVALUATION REPORT
**Project Title**: Krishi Mitra — ML-Powered Agricultural Intelligence Platform  
**Target Submission**: Faculty Mentor / College Evaluation Committee / Viva Voce  
**Live Web Application**: [https://krishi-mitra-nu.vercel.app/](https://krishi-mitra-nu.vercel.app/)  
**Repository**: [https://github.com/shresth1603/Krishi_Mitra](https://github.com/shresth1603/Krishi_Mitra)  
**Academic Year**: 2026  

---

## 📌 QUICK EXECUTIVE SUMMARY (For Evaluator & Viva)

| Question / Parameter | Verified Project Fact | Exact Codebase Reference |
| :--- | :--- | :--- |
| **1. Total Plants / Crops in Disease Detection** | **14 Plants** (Apple, Blueberry, Cherry, Corn/Maize, Grape, Orange, Peach, Bell Pepper, Potato, Raspberry, Soybean, Squash, Strawberry, Tomato) | [`labels.json`](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/disease_detection/labels.json) |
| **2. Total Disease & Health Classes** | **38 Classes** (**26 Pathogen/Disease Classes** + **12 Healthy Classes**) | [`main.py:L155-L180`](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L155-L180) |
| **3. Disease Model & Architecture** | MobileNetV2 Deep Learning Transfer Learning (Input: 224×224×3 RGB, Keras HDF5) | [`krishi_model.h5`](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/model/krishi_model.h5) (10.1 MB) |
| **4. Crop Recommendation Model** | Random Forest Classifier (200 trees, 7 features: NPK, pH, Temp, Humidity, Rain) | [`crop_rf_model.joblib`](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/models/crop_recommendation/crop_rf_model.joblib) |
| **5. Crop Recommendation Accuracy** | **99.55% Test Accuracy**, **99.59% ± 0.27% 5-Fold Cross-Validation** (22 Indian Crops) | [`metadata.json`](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/models/crop_recommendation/metadata.json) |
| **6. Mandi Price Data Source** | Official Government of India Portal: `data.gov.in` (AGMARKNET Resource API) | [`main.py:L1057-L1110`](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L1057-L1110) |
| **7. Marketplace / Order Type Status** | **Informational Dashboard ONLY**. No direct buying/selling (0 orders processed). F2C/F2B is planned on a 4-phase roadmap. | [`MARKETPLACE_AND_ORDER_FLOW.md`](file:///c:/Users/DELL/Krishi-Mitra-1/docs/MARKETPLACE_AND_ORDER_FLOW.md) |
| **8. Farmer Schemes Source** | 100% Official Central & State Government Portals (`pmkisan.gov.in`, `agriculture.up.gov.in`) | [`schemes.js`](file:///c:/Users/DELL/Krishi-Mitra-1/src/data/schemes.js) |
| **9. Automated Test Verification** | **41 / 41 Tests Passing (100% Pass Rate)** | [`test_backend.py`](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/tests/test_backend.py) |

---

## 1. DATASETS USED — KHA HAIN, KAB KE HAIN, AUR KYA HAIN?

### A. Plant Disease Detection Dataset (PlantVillage)
- **Dataset Name**: PlantVillage Benchmark Leaf Dataset
- **Official Source**: 
  - TensorFlow Datasets: [https://www.tensorflow.org/datasets/catalog/plant_village](https://www.tensorflow.org/datasets/catalog/plant_village)
  - Mendeley Data: [https://data.mendeley.com/datasets/tywbtsjrjv/1](https://data.mendeley.com/datasets/tywbtsjrjv/1)
  - Research Paper: Hughes & Salathé (2015), Penn State & EPFL.
- **Kha Hai (Local Path in Project)**:
  - Trained weights: `krishi-backend/model/krishi_model.h5` (10.15 MB)
  - Mobile format: `krishi-backend/model/krishi_model.tflite` (9.05 MB)
  - Class mappings: `krishi-backend/disease_detection/labels.json`
  - *(Note: Raw images of 800+ MB are excluded from Git repository to keep code lightweight).*
- **Kab Ka Hai (Time Period)**: Original publication 2015–2016; pre-trained weights integrated October 2026.
- **License**: Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0).
- **Environment**: Controlled laboratory/greenhouse conditions with uniform grey/black backgrounds.
- **Future Proposed Indian Dataset**: [AIKosh ICAR Rice & Maize Dataset](https://aikosh.indiaai.gov.in/home/datasets/details/crop_disease_and_insect_pest_image_dataset_for_rice_and_maize.html) (926 MB, authentic open-field photos from ICAR).

---

### B. Crop Recommendation Dataset
- **Dataset Name**: Agricultural Crop Recommendation Dataset
- **Official Source**: Kaggle / FAO-adjacent agricultural research benchmark ([https://www.kaggle.com/datasets/siddharthss/crop-recommendation-dataset](https://www.kaggle.com/datasets/siddharthss/crop-recommendation-dataset))
- **Kha Hai (Local Path in Project)**:
  - CSV file: `krishi-backend/datasets/crop_recommendation/Crop_recommendation.csv` (150 KB)
  - Trained model: `krishi-backend/models/crop_recommendation/crop_rf_model.joblib` (7.28 MB)
  - Normalization Scaler: `krishi-backend/models/crop_recommendation/crop_scaler.joblib`
  - Label Encoder: `krishi-backend/models/crop_recommendation/crop_label_encoder.joblib`
  - Metrics & Config: `krishi-backend/models/crop_recommendation/metadata.json`
- **Total Rows & Records**: Exactly **2,200 rows** (perfectly balanced: 100 rows for each of the 22 crops).
- **Kab Ka Hai (Time Period)**: Compiled benchmark dataset (2020–2022); trained and verified October 2026.
- **License**: Creative Commons Attribution 3.0 IGO (CC BY 3.0 IGO).
- **Input Features (7 Features)**:
  1. `N` (Nitrogen): 0 – 140 kg/ha
  2. `P` (Phosphorus): 5 – 145 kg/ha
  3. `K` (Potassium): 5 – 205 kg/ha
  4. `temperature`: 8.8°C – 43.7°C
  5. `humidity`: 14.3% – 99.9%
  6. `ph`: 3.5 – 9.9
  7. `rainfall`: 20.2 mm – 298.6 mm
- **Target Output (22 Crops)**: Apple, Banana, Blackgram, Chickpea, Coconut, Coffee, Cotton, Grapes, Jute, Kidneybeans, Lentil, Maize, Mango, Mothbeans, Mungbean, Muskmelon, Orange, Papaya, Pigeonpeas, Pomegranate, Rice, Watermelon.

---

### C. Mandi Wholesale Prices Dataset
- **Dataset Name**: Current Daily Price of Various Commodities from Various Markets (Mandi)
- **Official Source**: Open Government Data Platform India (`data.gov.in`)
- **Official Catalog URL**: [https://www.data.gov.in/catalog/current-daily-price-various-commodities-various-markets-mandi](https://www.data.gov.in/catalog/current-daily-price-various-commodities-various-markets-mandi)
- **Authority**: AGMARKNET, Directorate of Marketing & Inspection (DMI), Ministry of Agriculture and Farmers Welfare.
- **Resource API**: `https://api.data.gov.in/resource/9ef84268-d588-465a-a308-a864a43d0070`
- **Kha Hai (Implementation in Project)**:
  - Backend API client: `krishi-backend/main.py` (lines 1040–1150)
  - Time-series forecasting engine: `krishi-backend/mandi_forecast/`
- **Kab Ka Hai (Time Period)**: **Real-time daily spot data** (refreshed daily by APMC mandis across India).
- **Fields Provided**: State, District, Market (Mandi), Commodity, Variety, Arrival Date, Min Price, Max Price, Modal Price (₹/Quintal).

---

## 2. PLANTS & DISEASES — KITNE PLANTS AUR KAUN SI DISEASES HAIN?

The computer vision model classifies **38 distinct categories** covering **14 plants**: **26 disease states** and **12 healthy states**.

| S.No | Crop / Plant (14 Plants) | Diseases Identified (26 Pathogens) | Healthy Class (12 Classes) | Total |
| :---: | :--- | :--- | :--- | :---: |
| 1 | **Apple** | 1. Apple Scab<br/>2. Black Rot<br/>3. Cedar Apple Rust | 4. Apple Healthy | 4 |
| 2 | **Blueberry** | *(No disease class in benchmark)* | 5. Blueberry Healthy | 1 |
| 3 | **Cherry** | 6. Powdery Mildew | 7. Cherry Healthy | 2 |
| 4 | **Corn (Maize)** | 8. Cercospora / Gray Leaf Spot<br/>9. Common Rust<br/>10. Northern Leaf Blight | 11. Corn Healthy | 4 |
| 5 | **Grape** | 12. Black Rot<br/>13. Esca (Black Measles)<br/>14. Leaf Blight (Isariopsis Spot) | 15. Grape Healthy | 4 |
| 6 | **Orange** | 16. Haunglongbing (Citrus Greening) | *(No healthy class in benchmark)* | 1 |
| 7 | **Peach** | 17. Bacterial Spot | 18. Peach Healthy | 2 |
| 8 | **Pepper (Bell)** | 19. Bacterial Spot | 20. Pepper Bell Healthy | 2 |
| 9 | **Potato** | 21. Early Blight<br/>22. Late Blight | 23. Potato Healthy | 3 |
| 10 | **Raspberry** | *(No disease class in benchmark)* | 24. Raspberry Healthy | 1 |
| 11 | **Soybean** | *(No disease class in benchmark)* | 25. Soybean Healthy | 1 |
| 12 | **Squash** | 26. Powdery Mildew | *(No healthy class in benchmark)* | 1 |
| 13 | **Strawberry** | 27. Leaf Scorch | 28. Strawberry Healthy | 2 |
| 14 | **Tomato** | 29. Bacterial Spot<br/>30. Early Blight<br/>31. Late Blight<br/>32. Leaf Mold<br/>33. Septoria Leaf Spot<br/>34. Spider Mites (Two-Spotted)<br/>35. Target Spot<br/>36. Tomato Yellow Leaf Curl Virus (TYLCV)<br/>37. Tomato Mosaic Virus (ToMV) | 38. Tomato Healthy | 10 |
| **Total** | **14 Plants** | **26 Disease Classes** | **12 Healthy Classes** | **38** |

---

## 3. GEOGRAPHIC AREA FOCUS — KHA KHA KE DATA PAR FOCUS KIYA HAIN?

### A. Real Data vs. Interface Selectors (Honest Academic Audit)
- **UI Dropdowns**: The frontend contains selectors for **All 28 Indian States** and over **200+ Districts**.
- **Underlying Data Reality**:
  1. **Plant Disease Images**: Collected under controlled laboratory conditions in the USA and Switzerland. Pathogen morphology (e.g. *Alternaria solani*) is biologically universal, but real Indian field background noise is an acknowledged limitation.
  2. **Crop Recommendation**: Calibrated to general tropical/subtropical agro-ecological zones (soil NPK, pH, and rainfall), not restricted to one state.
  3. **Mandi Prices**: Dynamic pan-India coverage via official AGMARKNET. If the API key is not configured, fallback state price ranges are provided for 10 states: **Maharashtra, Punjab, Uttar Pradesh, Karnataka, Kerala, West Bengal, Tamil Nadu, Gujarat, Rajasthan, Madhya Pradesh**.
  4. **Weather Telemetry**: GPS coordinates are hardcoded for **48 major cities/districts** (including Lucknow, Kanpur, Agra, Varanasi, Allahabad, Meerut, Ghaziabad, Gorakhpur in UP, and all 14 districts of Kerala). For any other district (e.g. Orai/Jalaun), OpenWeatherMap dynamic city name geocoding is used.

### B. Uttar Pradesh & Bundelkhand (Orai / Jalaun) Pilot Strategy
For a regional pilot deployment, **Bundelkhand (specifically Jalaun / Orai)** is the recommended deployment zone:
- **Agronomic Focus**: Bundelkhand black soil (*Kabar/Mar*) and alluvial loam (*Parua*).
- **Target Crops in Jalaun/Orai**: Chickpea (*Chana*), Pigeonpea (*Arhar*), Lentil (*Masoor*), Mustard (*Sarson*), Wheat (*Gehun*).
- **Local Institution**: **KVK Jalaun (ICAR-IISR, Orai)** for localized agronomic advisory validation.

---

## 4. MARKETPLACE & ORDER FLOW — KYA ORDER LIYA HAI? (B2C, C2C, ETC.)

### A. Clear Academic Status: Dashboard vs. Transactional Marketplace
> **Critical Viva Point**: Krishi Mitra is currently an **Agricultural Information & Price Intelligence Dashboard**, **NOT** an active e-commerce or commercial trading platform.  
> **Total Orders Placed**: **0 (Zero)**. No money or bank transactions are handled on the website.

### B. Business Model Breakdown for Agriculture:
1. **F2C (Farmer to Consumer)**: Farmer sells fresh produce directly to urban consumers. *(Status: Planned)*
2. **F2B (Farmer to Business)**: Farmer sells bulk harvest (e.g. 50 quintals wheat) directly to food processors or retail chains. *(Status: Planned)*
3. **B2B (Business to Business)**: Seed/fertilizer manufacturers sell wholesale lots to village retail dealers. *(Status: Planned)*
4. **B2C (Business to Consumer)**: Agri-input companies sell bio-fertilizers or tools to farmers. *(Status: Planned)*
5. **C2C (Consumer to Consumer / Farmer to Farmer)**: Farmers rent equipment (tractors, harvesters) to neighboring farmers. *(Status: Planned)*
6. **B2B2C**: Agri-tech platform aggregates produce from FPOs and distributes via local kirana shops. *(Status: Planned)*

### C. 4-Phase Roadmap for Marketplace:
- **Phase 1 (Current State)**: Verified daily AGMARKNET mandi spot prices & econometric price forecasting.
- **Phase 2 (Next Step)**: Farmer crop listing catalog & buyer inquiries via WhatsApp bridge.
- **Phase 3 (Transactions)**: Formal digital orders, UPI/Razorpay Escrow payment, AGMARK quality verification, and truck logistics.
- **Phase 4 (Fintech)**: Bank-integrated Kisan Credit scoring based on verified trade history.

---

## 5. GOVERNMENT SCHEMES FOR FARMERS — KHA SE LENI HAIN?

### A. 100% Verified Official Government Portals:
1. **PM-KISAN**: [https://pmkisan.gov.in/](https://pmkisan.gov.in/) (Central income support of ₹6,000/year).
2. **PM Fasal Bima Yojana (PMFBY)**: [https://pmfby.gov.in/](https://pmfby.gov.in/) (Comprehensive crop insurance against natural calamities).
3. **Kisan Credit Card (KCC)**: [https://www.rbi.org.in/](https://www.rbi.org.in/) (Concessional crop loans up to ₹3 Lakh at 4% interest).
4. **Soil Health Card Portal**: [https://soilhealth.dac.gov.in/](https://soilhealth.dac.gov.in/) (Free 12-parameter soil testing & fertilizer advisory).
5. **UP Agriculture Department Portal**: [https://agriculture.up.gov.in/](https://agriculture.up.gov.in/) (UP State DBT portal for seed and machinery subsidies).
6. **Ministry of Agriculture & Farmers Welfare**: [https://agriwelfare.gov.in/](https://agriwelfare.gov.in/) (Central schemes repository).
7. **ICAR (Indian Council of Agricultural Research)**: [https://icar.org.in/](https://icar.org.in/) (Scientific advisories and KVK network).

### B. Schemes Currently Integrated in Krishi Mitra (`src/data/schemes.js`):
- **Central Schemes**: PM-KISAN, PMFBY, Kisan Credit Card (KCC), Paramparagat Krishi Vikas Yojana (PKVY - Organic), MGNREGS Agriculture, Rashtriya Krishi Vikas Yojana (RKVY).
- **State Schemes**: Maharashtra Shetkari Sanman, Karnataka Raitha Siri, Odisha KALIA, Telangana Rythu Bandhu.
- **Key UP State Schemes to Prioritize**: UP Krishi Yantra Subsidy (40-50% on farm implements), UP Khet Talab Yojana (50% farm pond subsidy for Bundelkhand), and UP Solar Pump Yojana (PM-KUSUM 60% subsidy).

### C. Sensitive Data & Privacy Rule (DPDP Act, 2023):
Krishi Mitra operates under a **Strict Zero-Storage Policy for Sensitive Identifiers**:
- Krishi Mitra **never** stores Aadhaar numbers, bank account numbers, or land ownership papers (*Khatauni*).
- The app acts as an eligibility discovery engine. When a farmer clicks **"Apply on Official Portal"**, the app redirects directly to the certified `.gov.in` portal where authentication happens securely.
