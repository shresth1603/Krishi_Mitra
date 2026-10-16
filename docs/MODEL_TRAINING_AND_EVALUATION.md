# 🧠 Krishi Mitra — Machine Learning Training, Evaluation & Reproducibility Guide

> **Audit Standard**: Verified against Source Training Scripts, Saved Artifacts, and Automated Test Suites  
> **Audited Date**: October 2026  
> **Backend Directory**: `krishi-backend/`  

---

## 1. Machine Learning Portfolio Overview

Krishi Mitra incorporates three distinct machine learning systems engineered for specific agricultural domains:

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                        KRISHI MITRA ML MODEL SUITE                              │
├───────────────────────┬─────────────────────────┬───────────────────────────────┤
│ Model 1: Leaf Disease │ Model 2: Crop Recommender│ Model 3: Mandi Price Forecast │
│ MobileNetV2 (CNN)     │ Random Forest (Ensemble)│ Lag-Ridge + Seasonal Naive    │
│ 38 Crop-Pathogen Class│ 22 Indian Crop Classes  │ Daily Modal Price (₹/Quintal) │
│ Computer Vision (RGB) │ Tabular Soil & Climate  │ Chronological Time-Series     │
│ Keras / TensorFlow    │ Scikit-Learn / Joblib   │ Scikit-Learn / Pandas         │
└───────────────────────┴─────────────────────────┴───────────────────────────────┘
```

---

## 2. Model 1 — Plant Disease Detection (MobileNetV2)

### A. Architectural Specification
- **Base Architecture**: MobileNetV2 pre-trained on ImageNet.
- **Input Dimensions**: `(None, 224, 224, 3)` with float32 pixel values normalized to `[0.0, 1.0]`.
- **Classification Head**:
  - `GlobalAveragePooling2D()`
  - `BatchNormalization()`
  - `Dropout(rate=0.3)`
  - `Dense(units=256, activation='relu')`
  - `Dropout(rate=0.2)`
  - `Dense(units=38, activation='softmax')`
- **Loss Function**: Categorical Crossentropy.
- **Optimizer**: Adam (`learning_rate=1e-3` in Stage 1; `learning_rate=1e-4` in Stage 2 fine-tuning).

### B. Training Procedure ([disease_detection/train.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/disease_detection/train.py))
- **Data Augmentation**:
  - `RandomFlip("horizontal")`
  - `RandomRotation(factor=0.1)` (±10% rotation)
  - `RandomZoom(height_factor=0.1)` (±10% zoom)
- **Two-Stage Transfer Learning**:
  - **Stage 1 (Head Training)**: MobileNetV2 base model frozen (`base_model.trainable = False`). Head trained for up to 15 epochs with `EarlyStopping(patience=4)` and `ReduceLROnPlateau(factor=0.2, patience=2)`.
  - **Stage 2 (Fine-Tuning)**: Top 30 layers of MobileNetV2 un-frozen (`base_model.layers[-30:]`). Model trained with a reduced learning rate of `1e-4` to fine-tune high-level visual feature extractors.
- **Reproducible Command**:
  ```bash
  python krishi-backend/disease_detection/train.py --data_dir /path/to/PlantVillage --epochs 15 --batch_size 32 --seed 42
  ```

### C. Evaluation & Artifacts ([disease_detection/evaluate.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/disease_detection/evaluate.py))
- **Evaluation Command**:
  ```bash
  python krishi-backend/disease_detection/evaluate.py --model krishi-backend/model/krishi_model.h5 --test_dir /path/to/test_data
  ```
- **Saved Model Artifacts**:
  - `krishi-backend/model/krishi_model.h5`: 10,153,720 bytes (Keras HDF5 binary).
  - `krishi-backend/model/krishi_model.tflite`: 9,053,336 bytes (Edge/Mobile-quantized FlatBuffer binary).
  - `krishi-backend/disease_detection/labels.json`: 38 class mapping objects.
  - `krishi-backend/disease_detection/metadata.json`: Architecture parameters and field limitations.

---

## 3. Model 2 — Crop Recommendation (Random Forest)

### A. Architectural Specification
- **Algorithm**: `RandomForestClassifier` (Ensemble of 200 decision trees).
- **Hyperparameters**:
  - `n_estimators=200`
  - `random_state=42`
  - `class_weight='balanced'`
  - `n_jobs=-1` (Parallel CPU execution)
- **Feature Scaling**: `StandardScaler` fitted on training split:
  $$z = \frac{x - \mu}{\sigma}$$
- **Features (7)**: `N`, `P`, `K`, `temperature`, `humidity`, `ph`, `rainfall`.
- **Target (22 Classes)**: Apple, Banana, Blackgram, Chickpea, Coconut, Coffee, Cotton, Grapes, Jute, Kidneybeans, Lentil, Maize, Mango, Mothbeans, Mungbean, Muskmelon, Orange, Papaya, Pigeonpeas, Pomegranate, Rice, Watermelon.

### B. Benchmark Evaluation Metrics ([metadata.json](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/models/crop_recommendation/metadata.json))
- **Dataset Size**: Exactly 2,200 rows (100 per class).
- **Split Ratio**: 80% Train (1,760 rows) / 20% Test (440 rows), stratified.

| Model / Benchmark | Test Accuracy | Weighted Precision | Weighted Recall | Weighted F1-Score | 5-Fold Cross-Validation |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Baseline (Decision Tree)** | 97.95% | 0.9802 | 0.9795 | 0.9794 | 97.82% ± 0.65% |
| **Random Forest (200 Trees)**| **99.55%** | **0.9957** | **0.9955** | **0.9955** | **99.59% ± 0.27%** |

### C. Feature Importance Decomposition
Random Forest Gini-importance indicates the relative contribution of each feature to the recommendation split:

$$\sum_{i=1}^{7} \text{Importance}(f_i) = 1.0$$

```
Rainfall (mm)   [██████████████████████] 21.96%
Humidity (%)    [█████████████████████ ] 21.71%
Potassium (K)   [██████████████████    ] 18.08%
Phosphorus (P)  [███████████████       ] 15.13%
Nitrogen (N)    [██████████            ] 10.34%
Temperature (°C)[███████               ]  7.55%
Soil pH         [█████                 ]  5.23%
```

### D. Reproducible Training Command
```bash
python krishi-backend/crop_recommendation/train.py \
    --data krishi-backend/datasets/crop_recommendation/Crop_recommendation.csv \
    --out krishi-backend/models/crop_recommendation
```
- **Saved Model Artifacts**:
  - `krishi-backend/models/crop_recommendation/crop_rf_model.joblib`: 7,280,649 bytes.
  - `krishi-backend/models/crop_recommendation/crop_scaler.joblib`: 735 bytes.
  - `krishi-backend/models/crop_recommendation/crop_label_encoder.joblib`: 696 bytes.
  - `krishi-backend/models/crop_recommendation/metadata.json`: 1,572 bytes.

---

## 4. Model 3 — Mandi Price Forecasting Pipeline

### A. Mathematical Formulation
Agricultural commodity wholesale prices exhibit strong weekly market periodicity (e.g. mandi closures on Sundays, major auction days on Mondays). To model these dynamics, the pipeline implements two complementary approaches:

#### 1. Baseline Model: Seasonal Naive Forecaster
Predicts the modal price from exactly one season period (7 days) prior:
$$\hat{y}_{t+h} = y_{t+h - m \cdot \lfloor(h-1)/m + 1\rfloor}$$
where $m = 7$ (7-day periodicity).

#### 2. Machine Learning Model: Lag-Feature Ridge Regression
Constructs an auto-regressive feature matrix from historical price lags:
- Lags: $y_{t-1}, y_{t-2}, y_{t-3}, y_{t-7}$
- Rolling Statistic: 7-day rolling mean $\mu_{7}(t-1) = \frac{1}{7} \sum_{i=1}^{7} y_{t-i}$
- Calendar Feature: Day of the week $d_t \in \{0, \dots, 6\}$

The target price is predicted via L2-regularized linear regression:
$$\hat{y}_t = \mathbf{w}^T \mathbf{x}_t + b, \quad \min_{\mathbf{w}} \left( \|\mathbf{y} - \mathbf{X}\mathbf{w}\|_2^2 + \alpha \|\mathbf{w}\|_2^2 \right)$$
where $\alpha = 1.0$. Recursive multi-step forecasting iterates forward for $h \in [1, 30]$ days.

### B. Strict Anti-Leakage Chronological Splitting
Random shuffling of time series records destroys temporal order and causes massive data leakage. In [mandi_forecast/forecasting.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/mandi_forecast/forecasting.py):
```python
def chronological_split(series: pd.Series, test_ratio: float = 0.2):
    n = len(series)
    split_idx = int(n * (1.0 - test_ratio))
    train = series.iloc[:split_idx]
    test = series.iloc[split_idx:]
    return train, test
```
The test set contains only the most recent 20% of chronological dates; the training set contains only the earliest 80%.

### C. Forecast Evaluation Metrics
The pipeline computes:
- **Mean Absolute Error (MAE)**:
  $$\text{MAE} = \frac{1}{n} \sum_{i=1}^n |y_i - \hat{y}_i|$$
- **Root Mean Squared Error (RMSE)**:
  $$\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^n (y_i - \hat{y}_i)^2}$$
- **Mean Absolute Percentage Error (MAPE)**:
  $$\text{MAPE} = \frac{100\%}{n} \sum_{i=1}^n \left| \frac{y_i - \hat{y}_i}{y_i} \right|$$

Model selection automatically selects between `LagRidgeForecaster` and `SeasonalNaiveBaseline` based on whichever achieves the lowest MAE on the held-out test split.

---

## 5. Automated Pytest Verification Suite

All models, data transformations, preprocessing pipelines, and edge-case guards are validated by **41 automated tests** in [krishi-backend/tests/test_backend.py](file:///c:/Users/DELL/Krishi-Mitra-1/krishi-backend/tests/test_backend.py).

### Execution Command:
```bash
python -m pytest krishi-backend/tests/test_backend.py -v
```
### Test Groups & Results:
- `TestHealth` (2 tests): Endpoint health, mock mode reporting, service flags (`PASSED`)
- `TestClasses` (4 tests): 38 classes, structure, healthy flags (`PASSED`)
- `TestParseClassName` (3 tests): String parsing, crop/disease separation (`PASSED`)
- `TestAnalyzeEndpoint` (8 tests): 200 OK, confidence bounds, corrupt byte rejection, advisory non-empty, severity (`PASSED`)
- `TestAdvisoryEndpoint` (4 tests): Standalone advisory, detailed/short modes, missing field 422 (`PASSED`)
- `TestCropRecommendation` (7 tests): Valid recommendation, top alternatives, pH validation (rejects pH > 14 with 422), negative N rejection (422), metadata info (`PASSED`)
- `TestMandiEndpoint` (3 tests): Agmarknet proxy, mock fallback structure, data fields (`PASSED`)
- `TestChatEndpoint` (3 tests): Empty question rejection (400), valid chat response, required fields (`PASSED`)
- `TestPlantDiseaseRobustness` (2 tests): Corrupt image byte rejection, mock flag identification (`PASSED`)
- `TestMandiForecasting` (6 tests): Forecast status endpoint, horizon bounds validation (rejects <=0 or >30 with 422), structured `DATA_UNAVAILABLE` reporting, ingestion deduplication, chronological split zero-leakage, baseline/Ridge algorithmic evaluation (`PASSED`)
