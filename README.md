# Spatial Damage Analysis using gwlearn (Noto Peninsula)

This repository contains an exploratory analysis of earthquake-induced building damage using spatial data and machine learning.

The goal is to understand the limitations of standard (global) machine learning models on spatial datasets and to investigate whether geographically weighted approaches can better capture local variation.

---

## Problem Overview

The dataset contains building-level damage information along with hazard indicators and spatial geometry.

Key challenges observed:

- Severe class imbalance (~97% no damage vs ~3% damage)
- Strong spatial clustering (confirmed via Moran’s I)
- Significant variation across municipalities (spatial heterogeneity)

These characteristics violate the assumptions of standard machine learning models, which treat observations as independent.

---

## Approach

The analysis is structured in three stages:

### 1. Exploratory Data Analysis (EDA)

- Data cleaning and validation  
- Spatial visualization of damage patterns  
- Hazard-based analysis (tsunami, slope failure, fire)  
- Spatial weights construction (KNN)  
- Moran’s I computation  

**Key observation:**  
Damage is spatially clustered and not randomly distributed.

---

### 2. Global Modeling (Baseline)

Standard machine learning models are trained:

- Logistic Regression  
- Random Forest  
- HistGradientBoosting  
- Stacking and blending  

Techniques used:
- Group-based train/test split (to avoid spatial leakage)
- Class weighting
- Manual SMOTE
- Feature engineering (interaction + regional features)

**Result:**

All models fail to capture the minority class:

- F1 ≈ 0.017  
- PR-AUC ≈ 0.01  

Despite extensive tuning, performance remains poor.

**Interpretation:**  
The limitation is not just class imbalance — global models fail to capture spatial structure.

---

### 3. Spatial Modeling (gwlearn)

Geographically weighted models are explored:

- GW Logistic Regression  
- GW Random Forest (partial)  
- GW Gradient Boosting (partial)  

Key ideas:
- Local models are fitted using spatial neighbourhoods  
- Nearby observations influence predictions more than distant ones  

---

## Bandwidth Selection

Bandwidth determines the size of the local neighbourhood.

An interval-based search was attempted using a stratified subset of the data.

**Observation:**

- Bandwidth search is computationally expensive  
- Even with subsampling (~58k rows), execution time becomes very large  

The process was manually interrupted, and a fallback bandwidth (k = 200) was used for experimentation.

---

## Spatial Model Results

- GW Logistic Regression successfully fitted  
- Prediction rate ≈ 25%  

This indicates that:
- Local models could only be fitted in regions with sufficient minority samples  
- Many locations were skipped due to extreme imbalance  

More complex models (GW-RF, GW-GB):
- Were interrupted due to computational cost  

---

## Key Insights

- Global models fail even after handling imbalance  
- Damage patterns are spatially dependent  
- Model performance varies across regions  
- Local modeling is necessary but computationally expensive  

This highlights a gap between standard ML workflows and spatial data requirements.

---

## Current Status

This project is a work in progress.

Completed:
- EDA and spatial analysis  
- Global modeling baseline  
- Initial spatial modeling experiments  

In progress:
- Efficient bandwidth selection  
- Scaling spatial models  
- Improving local model coverage  

---

## Future Work

- Optimize bandwidth search (e.g., heuristic or adaptive strategies)  
- Explore scalable implementations of spatial models  
- Improve handling of extreme class imbalance in local models  

---

## Data

The dataset is not included in this repository due to size constraints.

It can be obtained from the original source (Noto Peninsula earthquake building damage dataset).
