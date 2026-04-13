# SAHE: Severity-Aware Hybrid Ensemble Framework for Predicting Road Accident Hotspots in Dhaka

<p align="center">
  <img src="https://img.shields.io/badge/Domain-Road%20Safety%20AI-red?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Accuracy-74.89%25-brightgreen?style=for-the-badge" />
  <img src="https://img.shields.io/badge/F1--Score-28.03%25-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/ROC--AUC-67.10%25-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Python-3.8%2B-yellow?style=for-the-badge&logo=python" />
  <img src="https://img.shields.io/badge/License-MIT-lightgrey?style=for-the-badge" />
</p>

---

## 📌 Overview

Road accidents in Dhaka, one of the world's most densely populated cities, pose a critical threat to public safety. Predicting **severity-based accident hotspots** is a challenging task due to complex spatial, temporal, and traffic-related factors — compounded by significant class imbalance in accident data.

This repository contains the official implementation of the **Severity-Aware Hybrid Ensemble (SAHE)** framework, which:

- Analyzes **~47,680 historical crash records** from Dhaka
- Engineers spatial, temporal, and severity-related features at the **location level**
- Defines hotspots using a **severity score** combining death and injury counts
- Evaluates three baseline models: **Random Forest**, **XGBoost**, and **CatBoost**
- Proposes **SAHE** — a weighted probability blending ensemble with F1-optimized thresholds

SAHE achieves **74.89% accuracy** and **28.03% F1-score**, outperforming all individual baselines while reducing false positives — making it more reliable for real-world road safety planning in Dhaka.

---

## 📄 Paper

**A Severity-Aware Hybrid Ensemble Framework for Predicting Road Accident Hotspots in Dhaka**

| Authors | Affiliation |
|---|---|
| Al Israfil Niloy | Dept. of Computer Science, American International University-Bangladesh |
| Bijoy Ahamed | Dept. of Computer Science, American International University-Bangladesh |
| Md. Maruf Hasan Chowdhury | Dept. of Computer Science, American International University-Bangladesh |
| S.M. Abid Hossain Mahid | Dept. of Computer Science, American International University-Bangladesh |
| Dr. Muhammad Hasibur Rashid Chayon | Dept. of Computer Science, American International University-Bangladesh |

---

## 🏗️ Framework Architecture

```
Historical Accident Records (~47,680 records)
               │
               ▼
┌──────────────────────────────────┐
│        Data Preprocessing        │
│  • Remove duplicates             │
│  • Parse date, time, location    │
│  • Standardize text columns      │
└───────────────┬──────────────────┘
                │
                ▼
┌──────────────────────────────────┐
│        Feature Engineering       │
│  • time_of_day (5 categories)    │
│  • is_weekend (binary)           │
│  • vehicle_group (grouped)       │
│  • Severity Score:               │
│    5×Deaths + 2×Injuries + 1     │
└───────────────┬──────────────────┘
                │
                ▼
┌──────────────────────────────────┐
│   Location-Level Aggregation     │
│  • accident_count                │
│  • total_deaths / total_injuries │
│  • avg_severity / total_severity │
└───────────────┬──────────────────┘
                │
                ▼
┌──────────────────────────────────┐
│  Severity-Based Hotspot Label    │
│  Top 10% by total_severity = 1   │
│  Rest = 0 (Non-hotspot)          │
└───────────────┬──────────────────┘
                │
                ▼
┌──────────────────────────────────────────────────┐
│         SAHE: Weighted Probability Blending       │
│                                                  │
│  Random Forest  ──(w=0.30)──┐                   │
│  XGBoost        ──(w=0.30)──┼──► Final Score     │
│  CatBoost       ──(w=0.40)──┘                   │
│                                                  │
│  Decision Threshold: 0.65 (F1-optimized)         │
└──────────────────────────────────────────────────┘
                │
                ▼
    Predicted Severity-Based Hotspots
```

---

## 📊 Results

### Model Performance Comparison

| Model | Accuracy (%) | Precision (%) | Recall (%) | F1-Score (%) | ROC-AUC (%) |
|---|---|---|---|---|---|
| Random Forest | 66.67 | 16.13 | 55.56 | 25.00 | 65.78 |
| CatBoost | 69.11 | 16.90 | 53.33 | 25.67 | 67.06 |
| XGBoost | 73.33 | 19.01 | 51.11 | 27.71 | 67.41 |
| **SAHE (Proposed)** | **74.89** | **19.64** | **48.89** | **28.03** | **67.10** |

### Key Improvements of SAHE over XGBoost (Best Baseline)
- ✅ **+1.56%** higher accuracy
- ✅ **+0.32%** higher F1-score
- ✅ **+0.63%** higher precision
- ✅ **Fewer false positives** — 90 vs competitors (critical for resource allocation)
- ✅ **22 true hotspots** correctly identified

> **Note:** The low precision values reflect the inherent class imbalance challenge (only ~10% of locations are hotspots). SAHE provides the best trade-off between hotspot detection and false-alarm reduction.


---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/Dhaka-Accident-Hotspot-SAHE.git
cd Dhaka-Accident-Hotspot-SAHE
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Dataset

The dataset contains ~47,680 road accident records from Dhaka (2007–2024), sourced from:

> Nur, Fernaz; Sahin Afridi. *Bangladesh Road Traffic Accident Dataset (2007–2024): Multi-source integration from ARI, BRTA, DMP, MP Unit.*

Download the dataset from [Kaggle](https://www.kaggle.com/) and update the path in Cell 1:

```python
DATA_PATH = "path/to/Modified_Accident_Dataset_final.xlsx"
```

### 4. Run the Notebook

```bash
jupyter notebook Dhaka_Accident_Hotspot_SAHE.ipynb
```

**Run cells in order:**
1. **Imports & Setup** – loads all libraries
2. **Data Loading** – reads the accident Excel dataset
3. **Preprocessing** – cleans columns, parses dates/times, extracts coordinates
4. **Feature Engineering** – creates time_of_day, is_weekend, vehicle_group, severity score
5. **EDA** – severity distributions, hotspot vs non-hotspot analysis
6. **Location Aggregation** – builds location-level dataset
7. **Hotspot Labeling** – top 10% severity quantile threshold
8. **Base Models** – Random Forest, XGBoost, CatBoost with threshold tuning
9. **SAHE (Proposed)** – weighted probability blending ensemble
10. **Evaluation** – ROC curves, PR curves, confusion matrix, F1 bar chart

---

## 🧪 Dataset Details

| Property | Value |
|---|---|
| Source | ARI, BRTA, DMP, MP Unit (Bangladesh) |
| Records | ~47,680 accident records |
| Coverage | Dhaka city roads, 2007–2024 |
| Key Features | Location, Date/Time, Vehicle Info, Death Count, Injury Count, Weather, Light, Road Condition |
| Engineered Features | time_of_day, is_weekend, vehicle_group, severity_score |
| Prediction Level | Location-level (aggregated) |
| Hotspot Definition | Top 10% of locations by total severity score |
| Train / Test Split | 80% / 20% (stratified) |

---

## ⚖️ Severity Score Formula

Each accident is scored as:

```
Severity Score = 5 × Death Count + 2 × Injury Count + 1
```

Locations are then ranked by their aggregated total severity score, and the **top 10%** are labeled as **hotspots**.

---

## ⚙️ SAHE Ensemble Weights

```
Final Score = 0.30 × P(RF) + 0.30 × P(XGB) + 0.40 × P(CatBoost)

Decision Threshold = 0.65  (F1-optimized)
```

CatBoost receives a slightly higher weight (0.40) due to its stronger performance on imbalanced data with categorical features.

---

## 📬 Citation

If you use this code or find this work helpful, please cite:

```bibtex
@article{niloy2026sahe,
  title   = {A Severity-Aware Hybrid Ensemble Framework for Predicting
             Road Accident Hotspots in Dhaka},
  author  = {Niloy, Al Israfil and Ahamed, Bijoy and Chowdhury, Md. Maruf Hasan
             and Mahid, S.M. Abid Hossain and Chayon, Dr. Muhammad Hasibur Rashid},
  journal = {American International University-Bangladesh},
  year    = {2026}
}
```

---

## 🙏 Acknowledgements

This research was conducted at the **Department of Computer Science, American International University-Bangladesh**.

The accident dataset was sourced from ARI, BRTA, DMP, and MP Unit records compiled by Nur, Fernaz & Sahin Afridi.

---

## 📜 License

This project is licensed under the MIT License. See [LICENSE](./LICENSE) for details.
