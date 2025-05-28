# 🌲 Detecting Tree Mortality from Aerial Imagery

This repository contains the complete pipeline for classifying individual pixels in NAIP aerial imagery as **LIVE**, **DEAD**, or **BARE** vegetation using spectral features (RGB, NIR, NDVI).

The current version supports:
- Preprocessing and spectral normalization across years
- Manual labeling of sampled pixels
- Random Forest training with NDVI enhancement
- Full-raster predictions for 2014–2022
- Temporal trend analysis of class distributions

Built and maintained by [Dev Paragiri](https://deveshparagiri.com).

---

## 🗂️ Project Structure

```
TREE-MORTALITY-V3/
├── assets/                     # Exported plots and visualizations
├── env/                        # Local Python environment (not tracked)
├── labeled_samples/            # TIF+PNG crops for labeled pixels
├── labels/                     # Label CSVs (e.g. labeled2.csv)
├── models/                     # Trained Random Forest models
├── naip_data/                  # Original NAIP imagery (raw RGB/NIR)
├── ndvi_data/                  # NDVI .npy or .tif outputs (intermediate)
├── predicted_rasters/          # Model predictions (per year)
├── preprocessed_output_{year}/ # Aligned, normalized rasters for each year
├── .gitignore
├── README.md
├── preprocess.ipynb            # Spectral norm + NDVI calculation
├── process.ipynb               # Label extraction + sample creation
├── model.ipynb                 # Train + evaluate RF model
├── stats.ipynb                 # Analysis, histograms, trend plots
├── increment.ipynb             # Chi-squared test, misc experiments
```


---

## ⚙️ Setup

```bash
git clone https://github.com/devparagiri/tree-mortality-v3.git
cd tree-mortality-v3
python -m venv env && source env/bin/activate
pip install -r requirements.txt
```

⸻

## 🔁 Workflow Overview

### 1. Preprocess imagery

Run preprocess.ipynb to:
	•	Align NAIP images spatially
	•	Normalize bands (R, G, B, NIR)
	•	Generate NDVI-enhanced outputs

### 2. Label pixels

Run process.ipynb to:
	•	Sample pixels
	•	Export TIFs and PNG previews
	•	Populate labeled2.csv with human-verified class labels

### 3. Train model

Run model.ipynb to:
	•	Train a Random Forest on [R, G, B, NIR, NDVI]
	•	Use class_weight="balanced" to handle label imbalance
	•	Save model to models/

### 4. Predict across years

Use:
```python

from predict_rasters import predict_raster_for_year

for y in [2014, 2016, 2018, 2020, 2022]:
    predict_raster_for_year(y)
```

### 5. Analyze results

Run stats.ipynb and increment.ipynb to:
	•	Plot NDVI distributions
	•	Generate confusion matrix
	•	Analyze temporal class shifts
	•	Run chi-squared statistical tests

---

## 📊 Outputs
	•	predicted_rasters/: .tif raster maps of predicted class per pixel
	•	assets/: line plots, bar charts, confusion matrices, histograms
	•	models/: trained .joblib model with 5 features (RGB, NIR, NDVI)

---

## 🧠 Key Features
	•	Supports 5-band pixel inputs: Red, Green, Blue, NIR, NDVI
	•	Trains on as few as ~100 labeled pixels/class
	•	Accurate per-year generalization (validated on held-out data)
	•	Temporal trend analysis of vegetation change from 2014–2022

---

## 📬 Author

Dev Paragiri
[LinkedIn](https://www.linkedin.com/in/deveshparagiri) | [X](https://x.com/deveshparagiri)