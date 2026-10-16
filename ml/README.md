# 🧠 Krishi Mitra — Machine Learning (ML) Core

This directory contains the complete machine learning pipelines, trained model artifacts, datasets, feature engineering, and evaluation scripts for the Krishi Mitra agricultural platform.

---

## 📁 Directory Structure

```text
ml/
├── crop_recommendation/              # ML Model 2: Tabular Crop Recommendation
│   ├── __init__.py
│   ├── train.py                      # Random Forest training script (99.55% accuracy)
│   └── inference.py                  # Runtime inference & input range validation
│
├── disease_detection/                # ML Model 1: Plant Disease Detection (38 Classes)
│   ├── __init__.py
│   ├── train.py                      # MobileNetV2 transfer learning training pipeline
│   ├── evaluate.py                   # Test dataset evaluation (accuracy, F1, confusion matrix)
│   ├── labels.json                   # 38 PlantVillage crop-pathogen category definitions
│   └── metadata.json                 # Input dimensions (224x224x3), preprocessing, limits
│
├── mandi_forecast/                   # ML Model 3: Mandi Price Forecasting
│   ├── __init__.py
│   ├── ingestion.py                  # Schema normalization, date parsing & deduplication
│   ├── forecasting.py                # Chronological splitting, Seasonal Naive & Lag-Ridge models
│   └── service.py                    # High-level coordinator & anti-fabrication data guard
│
├── datasets/                         # Datasets directory
│   ├── crop_recommendation/
│   │   └── Crop_recommendation.csv   # 2,200 rows, 22 crops, CC BY 3.0 IGO
│   └── mandi_historical/             # Target folder for verified AGMARKNET CSV archives
│
└── models/                           # Trained Model Weights & Artifacts
    ├── plant_disease/
    │   ├── krishi_model.h5           # Keras HDF5 model weights (10.15 MB)
    │   └── krishi_model.tflite       # Edge/Mobile-quantized TFLite binary (9.05 MB)
    └── crop_recommendation/
        ├── crop_rf_model.joblib      # Fitted Random Forest Classifier (200 trees, 7.28 MB)
        ├── crop_scaler.joblib        # Fitted StandardScaler (735 bytes)
        ├── crop_label_encoder.joblib # Fitted LabelEncoder (696 bytes)
        └── metadata.json             # 99.55% accuracy metrics & training configuration
```

---

## 🚀 Training & Evaluation Commands

### 1. Train Crop Recommendation Model (Model 2)
```bash
python ml/crop_recommendation/train.py \
    --data ml/datasets/crop_recommendation/Crop_recommendation.csv \
    --out ml/models/crop_recommendation
```
*Outputs: 99.55% test accuracy, 99.59% ± 0.27% 5-fold cross-validation.*

### 2. Train Plant Disease Detection Model (Model 1)
```bash
python ml/disease_detection/train.py \
    --data_dir /path/to/PlantVillage \
    --output_model ml/models/plant_disease/krishi_model.h5 \
    --epochs 15
```

### 3. Evaluate Plant Disease Model
```bash
python ml/disease_detection/evaluate.py \
    --model ml/models/plant_disease/krishi_model.h5 \
    --test_dir /path/to/test_data
```

### 4. Run Mandi Forecasting Pipeline Unit Tests
```bash
python -m pytest krishi-backend/tests/test_backend.py -k "TestMandiForecasting" -v
```
