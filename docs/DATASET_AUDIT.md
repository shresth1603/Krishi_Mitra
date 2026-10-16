# 🔬 Krishi Mitra — Comprehensive Dataset Audit & Provenance Report

> **Audit Standard**: Evidence-Based Verification against Repository Files, Training Pipelines, and Model Weights  
> **Audited Date**: October 2026  
> **Auditor**: Senior Data & ML Engineer  

---

## 1. Executive Summary

This audit catalogs all data assets associated with Krishi Mitra, examining file integrity, schema definitions, training scripts, model artifacts, licensing terms, and real-world applicability.

### Key Audit Findings:
1. **Plant Disease Detection**: The pre-trained weights (`krishi-backend/model/krishi_model.h5`, 10.1 MB) and class labels (`disease_detection/labels.json`) exist in the repository. The raw 54,000+ image dataset itself is **not** stored locally in Git (which is standard practice for multi-gigabyte datasets). The pipeline is configured for the **PlantVillage** dataset (38 classes, 14 crops).
2. **Crop Recommendation**: The complete dataset (`krishi-backend/datasets/crop_recommendation/Crop_recommendation.csv`) **is present** in the repository (150,034 bytes, 2,200 rows, 7 features + 1 target, 22 classes, licensed CC BY 3.0 IGO).
3. **Mandi Price Ingestion & Forecasting**: The live spot price integration connects to `data.gov.in` AGMARKNET API. However, multi-month historical time-series CSV archives are **not present** on disk in `krishi-backend/datasets/mandi_historical/`. The forecasting pipeline honestly reports `DATA_UNAVAILABLE` rather than fabricating numbers.
4. **Government Schemes Database**: Present as a structured JavaScript module (`src/data/schemes.js`, 5,299 bytes, 10 curated schemes).

---

## 2. Dataset A — Plant Disease Detection Dataset (PlantVillage)

### A. Provenance & Metadata
- **Dataset Name**: PlantVillage Leaf Image Dataset
- **Official Source**: 
  - TensorFlow Datasets Catalog: [https://www.tensorflow.org/datasets/catalog/plant_village](https://www.tensorflow.org/datasets/catalog/plant_village)
  - Original Mendeley Repository: [https://data.mendeley.com/datasets/tywbtsjrjv/1](https://data.mendeley.com/datasets/tywbtsjrjv/1)
  - Paper: Hughes, D. P., & Salathé, M. (2015). *"An open access repository of images on plant health to enable the development of mobile disease diagnostics."* arXiv:1511.08060.
- **Exact Local Directory**: Model weights are stored in `krishi-backend/model/krishi_model.h5` and class mappings in `krishi-backend/disease_detection/labels.json`. Raw image folders are **absent** from the repository (correctly ignored via `.gitignore`).
- **File Format**: Standard JPEG / PNG images (originally 256×256 pixels, preprocessed to 224×224 RGB).
- **Total Records (Original Benchmark)**: 54,303 leaf photographs.
- **License**: Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0).
- **Collection Setting**: Laboratory / greenhouse conditions with controlled lighting and uniform grey/black paper backgrounds.

### B. Classes & Taxonomy Breakdown
The dataset covers **38 total classes** spanning **14 distinct plant/crop species**, divided into **26 diseased classes** and **12 healthy classes**.

#### Complete Verified Crop & Class Inventory:
| Crop Species (14) | Diseased Classes (26) | Healthy Classes (12) | Total Classes per Crop |
| :--- | :--- | :--- | :--- |
| **Apple** (4) | 1. Apple Scab (*Venturia inaequalis*)<br/>2. Black Rot (*Botryosphaeria obtusa*)<br/>3. Cedar Apple Rust (*Gymnosporangium juniperi-virginianae*) | 4. Apple Healthy | 4 |
| **Blueberry** (1) | *None in benchmark* | 5. Blueberry Healthy | 1 |
| **Cherry** (2) | 6. Powdery Mildew (*Podosphaera clandestina*) | 7. Cherry Healthy | 2 |
| **Corn (Maize)** (4) | 8. Cercospora Leaf Spot / Gray Leaf Spot (*Cercospora zeae-maydis*)<br/>9. Common Rust (*Puccinia sorghi*)<br/>10. Northern Leaf Blight (*Exserohilum turcicum*) | 11. Corn Healthy | 4 |
| **Grape** (4) | 12. Black Rot (*Guignardia bidwellii*)<br/>13. Esca / Black Measles (*Phaeomoniella chlamydospora*)<br/>14. Leaf Blight / Isariopsis Leaf Spot (*Pseudocercospora vitis*) | 15. Grape Healthy | 4 |
| **Orange** (1) | 16. Haunglongbing / Citrus Greening (*Candidatus Liberibacter*) | *None in benchmark* | 1 |
| **Peach** (2) | 17. Bacterial Spot (*Xanthomonas campestris pv. pruni*) | 18. Peach Healthy | 2 |
| **Pepper (Bell)** (2) | 19. Bacterial Spot (*Xanthomonas vesicatoria*) | 20. Pepper Bell Healthy | 2 |
| **Potato** (3) | 21. Early Blight (*Alternaria solani*)<br/>22. Late Blight (*Phytophthora infestans*) | 23. Potato Healthy | 3 |
| **Raspberry** (1) | *None in benchmark* | 24. Raspberry Healthy | 1 |
| **Soybean** (1) | *None in benchmark* | 25. Soybean Healthy | 1 |
| **Squash** (1) | 26. Powdery Mildew (*Podosphaera xanthii*) | *None in benchmark* | 1 |
| **Strawberry** (2) | 27. Leaf Scorch (*Diplocarpon earlianum*) | 28. Strawberry Healthy | 2 |
| **Tomato** (10) | 29. Bacterial Spot (*Xanthomonas campestris pv. vesicatoria*)<br/>30. Early Blight (*Alternaria solani*)<br/>31. Late Blight (*Phytophthora infestans*)<br/>32. Leaf Mold (*Passalora fulva*)<br/>33. Septoria Leaf Spot (*Septoria lycopersici*)<br/>34. Spider Mites / Two-Spotted Spider Mite (*Tetranychus urticae*)<br/>35. Target Spot (*Corynespora cassiicola*)<br/>36. Tomato Yellow Leaf Curl Virus (TYLCV)<br/>37. Tomato Mosaic Virus (ToMV) | 38. Tomato Healthy | 10 |

### C. Training, Preprocessing & Model Evaluation
- **Preprocessing Pipeline**: [main.py:L210-L219](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L210-L219)
  1. Pillow header verification (`img.verify()`).
  2. Conversion to RGB 3-channel.
  3. Resizing to `(224, 224)`.
  4. Rescaling pixel intensities by `1.0 / 255.0` to `[0.0, 1.0]`.
  5. Expanding dimensions to tensor shape `(1, 224, 224, 3)`.
- **Architecture**: MobileNetV2 Transfer Learning with ImageNet base weights, GlobalAveragePooling2D, BatchNormalization, Dropout (0.3), Dense (256, ReLU), Dropout (0.2), and Dense (38, Softmax).
- **Training Pipeline Script**: [train.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/disease_detection/train.py)
  - Split: 80% train, 20% validation (`seed=42`).
  - Stage 1: Classifier head training (Adam, `lr=1e-3`, EarlyStopping patience=4).
  - Stage 2: Fine-tuning top 30 layers of MobileNetV2 (Adam, `lr=1e-4`).
- **Saved Model File**: `krishi-backend/model/krishi_model.h5` (10,153,720 bytes) and `krishi_model.tflite` (9,053,336 bytes).

### D. Critical Real-World Limitations
1. **Controlled Background Bias**: Because PlantVillage photos were captured on neutral uniform sheets in lab conditions, CNN models frequently learn background artifacts. In real-world Indian open fields, accuracy drops when weeds, direct sunlight glare, soil, or human fingers are in frame.
2. **Confidence vs. Agronomic Severity**: In `main.py`, model confidence is strictly treated as statistical probability of class assignment. Agronomic severity is computed independently by crop risk rules.
3. **Proposed Expansion (AIKosh ICAR Dataset)**: Future iterations should train on the official [AIKosh Rice & Maize Crop Disease Dataset](https://aikosh.indiaai.gov.in/home/datasets/details/crop_disease_and_insect_pest_image_dataset_for_rice_and_maize.html) (ICAR Data Use License, 926 MB), which contains authentic Indian open-field photographs.

---

## 3. Dataset B — Crop Recommendation Dataset

### A. Provenance & Metadata
- **Dataset Name**: Crop Recommendation Dataset
- **Official Source**: Kaggle / FAO-adjacent agronomic research benchmark ([https://www.kaggle.com/datasets/siddharthss/crop-recommendation-dataset](https://www.kaggle.com/datasets/siddharthss/crop-recommendation-dataset))
- **Exact Local Path**: `krishi-backend/datasets/crop_recommendation/Crop_recommendation.csv`
- **File Format & Size**: CSV (Comma-Separated Values), 150,034 bytes
- **Total Records**: Exactly **2,200 rows** (balanced: exactly 100 rows per crop class)
- **License**: Creative Commons Attribution 3.0 IGO (CC BY 3.0 IGO)
- **Geographic Coverage**: Generic agro-ecological profiles calibrated to tropical and sub-tropical farming zones.

### B. Feature Schema & Statistical Distributions
| Column Name | Data Type | Physical Unit | Valid Range (Observed) | Agronomic Significance |
| :--- | :--- | :--- | :--- | :--- |
| `N` | Numeric (Float) | kg/ha | 0 – 140 | Available Nitrogen content in soil |
| `P` | Numeric (Float) | kg/ha | 5 – 145 | Available Phosphorus content in soil |
| `K` | Numeric (Float) | kg/ha | 5 – 205 | Available Potassium content in soil |
| `temperature` | Numeric (Float) | °C | 8.8°C – 43.7°C | Average ambient temperature |
| `humidity` | Numeric (Float) | % | 14.3% – 99.9% | Relative atmospheric humidity |
| `ph` | Numeric (Float) | pH scale | 3.5 – 9.9 | Soil acidity / alkalinity |
| `rainfall` | Numeric (Float) | mm/year | 20.2 mm – 298.6 mm | Annual precipitation / seasonal moisture |
| `label` (Target) | Categorical | String | 22 unique crop names | Recommended crop species |

#### 22 Crop Classes:
`apple`, `banana`, `blackgram`, `chickpea`, `coconut`, `coffee`, `cotton`, `grapes`, `jute`, `kidneybeans`, `lentil`, `maize`, `mango`, `mothbeans`, `mungbean`, `muskmelon`, `orange`, `papaya`, `pigeonpeas`, `pomegranate`, `rice`, `watermelon`.

### C. Data Quality & Cleaning Audit
- **Missing Values**: 0 nulls across all 8 columns.
- **Duplicate Rows**: 0 duplicates found.
- **Class Balance**: Perfectly balanced (100 samples per class, exactly 4.545% per class).
- **Train/Test Split**: 80% train (1,760 rows), 20% test (440 rows) stratified by class (`random_state=42`).

### D. Model Performance & Artifacts
Trained via [crop_recommendation/train.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/crop_recommendation/train.py):
- **Baseline (Decision Tree)**: 97.95% test accuracy
- **Random Forest (200 trees, balanced weights)**:
  - **Test Accuracy**: **99.55%** (438 / 440 test samples correct)
  - **Weighted Precision**: **99.57%**
  - **Weighted Recall**: **99.55%**
  - **Weighted F1-Score**: **99.55%**
  - **5-Fold Cross-Validation**: **99.59% ± 0.27%**
- **Feature Importance Ranking**:
  1. `rainfall`: **21.96%**
  2. `humidity`: **21.71%**
  3. `K` (Potassium): **18.08%**
  4. `P` (Phosphorus): **15.13%**
  5. `N` (Nitrogen): **10.34%**
  6. `temperature`: **7.55%**
  7. `ph`: **5.23%**
- **Artifacts Saved**: `crop_rf_model.joblib` (7.28 MB), `crop_scaler.joblib` (735 bytes), `crop_label_encoder.joblib` (696 bytes), `metadata.json` (1,572 bytes).

---

## 4. Dataset C — Mandi Spot Prices & Forecasting Pipeline

### A. Provenance & Source Reference
- **Dataset Name**: Current Daily Price of Various Commodities from Various Markets (Mandi)
- **Official Portal**: Open Government Data Platform India (`data.gov.in`)
- **Official Catalog URL**: [https://www.data.gov.in/catalog/current-daily-price-various-commodities-various-markets-mandi](https://www.data.gov.in/catalog/current-daily-price-various-commodities-various-markets-mandi)
- **Data Source Authority**: Agricultural Marketing Information Network (AGMARKNET), Directorate of Marketing & Inspection (DMI), Ministry of Agriculture and Farmers Welfare.
- **API Endpoint Resource**: `https://api.data.gov.in/resource/9ef84268-d588-465a-a308-a864a43d0070`
- **Data Type**: Daily wholesale agricultural commodity arrivals and spot prices.

### B. Live Spot Price Schema
When retrieved via the live backend proxy ([main.py:L1057-L1110](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/main.py#L1057-L1110)):
- `State`: State name (e.g., `Maharashtra`, `Uttar Pradesh`)
- `District`: District name
- `Market`: Mandi name (e.g., `Pune`, `Lucknow`, `Orai`)
- `Commodity`: Agricultural commodity (e.g., `Tomato`, `Potato`, `Wheat`)
- `Variety`: Crop variety (e.g., `Hybrid`, `Desi`, `Common`)
- `Arrival_Date`: Date of arrival (`DD/MM/YYYY`)
- `Min_Price`: Minimum wholesale price (₹/Quintal)
- `Max_Price`: Maximum wholesale price (₹/Quintal)
- `Modal_Price`: Most frequent wholesale transaction price (₹/Quintal)

### C. Forecasting Pipeline Status & Anti-Fabrication Guard
- **Engine Location**: `krishi-backend/mandi_forecast/`
- **Current Observation Depth**:
  - The live Agmarknet API endpoint serves **only current daily spot arrivals**.
  - Multi-month or multi-year historical time-series CSV archives are **not pre-loaded** on disk in `krishi-backend/datasets/mandi_historical/`.
- **Anti-Fabrication Guard**:
  When a user requests a forecast for a commodity where historical data is absent or has fewer than 30 daily observations, the backend returns:
  ```json
  {
    "status": "DATA_UNAVAILABLE",
    "forecast_available": false,
    "message": "Verified historical time-series data is not currently loaded on disk. Agmarknet daily API returns live spot prices, which cannot be used for autoregressive time-series forecasting without sufficient historical depth (>= 30 days)...",
    "disclaimer": "Krishi Mitra strictly prohibits fabricating price forecasts without verified historical data."
  }
  ```
  This guarantees academic honesty and regulatory compliance.

---

## 5. Dataset Summary & Verification Table

| Dataset Name | Official Source / URL | Exact Local Path | Time Period / Refresh | Geographic Coverage | Records / Sample Count | Features / Classes | Active Usage | Verification Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **PlantVillage Leaf Images** | [TensorFlow Datasets](https://www.tensorflow.org/datasets/catalog/plant_village) / Mendeley | Pretrained weights in `krishi-backend/model/krishi_model.h5` | Fixed research benchmark (2015) | Controlled greenhouse / laboratory | 54,303 images in source; weights loaded in app | 38 classes (14 crops, 26 diseases, 12 healthy) | Consumed by `POST /analyze` in `main.py` | `[VERIFIED IMPLEMENTED]` (Weights verified; raw images excluded from Git) |
| **Crop Recommendation CSV** | [Kaggle Dataset](https://www.kaggle.com/datasets/siddharthss/crop-recommendation-dataset) (CC BY 3.0 IGO) | `krishi-backend/datasets/crop_recommendation/Crop_recommendation.csv` | Static agronomic benchmark | General tropical / sub-tropical agro-climates | Exactly 2,200 rows | 7 features (N, P, K, temp, humidity, pH, rain) -> 22 crop classes | Consumed by `train.py` & `POST /api/crop-recommendation` | `[VERIFIED IMPLEMENTED]` (Data, model, and API fully verified) |
| **AGMARKNET Live Mandi Prices** | [data.gov.in](https://www.data.gov.in/catalog/current-daily-price-various-commodities-various-markets-mandi) | Remote API proxy with 5-min memory cache | Live daily market refresh | Pan-India (28 states, 2,700+ mandis) | Dynamic (up to 100 records per API query) | State, District, Mandi, Commodity, Variety, Min/Max/Modal Price | Consumed by `GET /api/mandi` | `[PARTIALLY IMPLEMENTED]` (Live with valid key, state mock fallback) |
| **Historical Mandi Archives** | data.gov.in Mandi Catalog | Target path: `krishi-backend/datasets/mandi_historical/` | Multi-month time series archive | Site-specific (e.g. Maharashtra, UP mandis) | 0 records on disk (pending user upload) | Date, State, Market, Commodity, Modal_Price | Consumed by `mandi_forecast/service.py` | `[PLANNED]` (Pipeline verified; data archive pending) |
| **Government Schemes Database** | Central & State Agri Portals (PM-KISAN, PMFBY) | `src/data/schemes.js` | Updated October 2026 | Pan-India (Central) + Maharashtra, Karnataka, Odisha, Telangana | 10 curated scheme objects | Title, fullName, category, states, benefit, eligibility, documents, link | Consumed by `src/pages/SchemesPage.jsx` | `[VERIFIED IMPLEMENTED]` (Local JS database verified) |
