# SpaceX Falcon 9 First Stage Landing Prediction

A end-to-end data science project that collects, processes, visualizes, and models SpaceX Falcon 9 launch data to predict whether the first stage booster will land successfully — and by extension, estimate the cost of a launch.

> SpaceX advertises Falcon 9 launches at ~$62M vs. competitors at $165M+. The key saving is first-stage reusability. Predicting a successful landing lets any company bid more accurately against SpaceX.

---

## Tech Stack

![Python](https://img.shields.io/badge/Python-3.8+-blue?logo=python)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-yellow?logo=scikit-learn)
![Folium](https://img.shields.io/badge/Folium-Maps-green)
![Pandas](https://img.shields.io/badge/Pandas-Data-lightblue?logo=pandas)

---

## Repository Structure

```
spacex-falcon9-landing-prediction/
│
├── webscraping_solved.ipynb               # Lab 1 – Data Collection via Web Scraping
├── api_data_collection_solved.ipynb       # Lab 2 – Data Collection via SpaceX REST API
├── eda_dataviz_solved.ipynb               # Lab 3 – Exploratory Data Analysis & Visualization
├── folium_solved.ipynb                    # Lab 4 – Interactive Map Visualization with Folium
├── ml_prediction_solved.ipynb             # Lab 5 – Machine Learning Classification Models
└── README.md
```

---

## Project Pipeline

```
Web Scraping ──┐
               ├──▶ Raw Data ──▶ EDA & Feature Engineering ──▶ ML Prediction
SpaceX API ────┘                        │
                                        └──▶ Folium Maps (Geospatial Analysis)
```

---

## Notebooks Overview

### Lab 1 — Web Scraping (`webscraping_solved.ipynb`)
Scrapes Falcon 9 and Falcon Heavy historical launch records from a Wikipedia snapshot using `BeautifulSoup`.

**Key tasks:**
- HTTP GET request to a static Wikipedia page
- Parse HTML tables to extract launch records
- Extract columns: Flight No., Date, Booster Version, Launch Site, Payload, Payload Mass, Orbit, Customer, Launch Outcome, Booster Landing
- Export to `spacex_web_scraped.csv`

**Libraries:** `requests`, `BeautifulSoup`, `pandas`, `unicodedata`

---

### Lab 2 — API Data Collection (`api_data_collection_solved.ipynb`)
Collects structured launch data from the SpaceX REST API (`api.spacexdata.com/v4`).

**Key tasks:**
- Fetch launch records from the SpaceX API
- Use helper functions to enrich data via sub-calls to rockets, launchpads, payloads, and cores endpoints
- Filter to Falcon 9 only (remove Falcon 1 records)
- Handle missing `PayloadMass` values by imputing the column mean
- Export to `dataset_part_1.csv`

**Libraries:** `requests`, `pandas`, `numpy`, `datetime`

---

### Lab 3 — EDA & Data Visualization (`eda_dataviz_solved.ipynb`)
Exploratory Data Analysis with feature engineering to prepare data for machine learning.

**Key tasks:**
- Visualize FlightNumber vs. LaunchSite and PayloadMass vs. LaunchSite
- Plot success rate by orbit type (bar chart)
- Visualize FlightNumber vs. Orbit and PayloadMass vs. Orbit
- Plot yearly launch success trend (line chart)
- One-hot encode categorical columns: Orbit, LaunchSite, LandingPad, Serial
- Cast all features to `float64`
- Export to `dataset_part_3.csv`

**Libraries:** `pandas`, `numpy`, `matplotlib`, `seaborn`

---

### Lab 4 — Interactive Map Visualization (`folium_solved.ipynb`)
Geospatial analysis of SpaceX launch sites using interactive Folium maps.

**Key tasks:**
- Mark all 4 launch sites on a world map with labeled circles
- Overlay color-coded success/failure markers using `MarkerCluster` (green = success, red = failure)
- Add `MousePosition` plugin to identify proximity coordinates
- Calculate and display distances from CCAFS LC-40 to:
  - Closest coastline (~0.97 km)
  - Nearest city — Cocoa Beach, FL (~28 km)
  - Nearest highway — US-1 (~20 km)
  - Nearest railway (~21 km)
- Draw `PolyLine` distance lines for each proximity

**Libraries:** `folium`, `pandas`

**Key findings:**
- All launch sites are within 1 km of the coastline (trajectories over water for safety)
- Sites are near the equator to leverage Earth's rotational velocity
- Cities are kept at a safe distance (> 20 km)

---

### Lab 5 — Machine Learning Prediction (`ml_prediction_solved.ipynb`)
Trains and compares four classification algorithms to predict first-stage landing success.

**Key tasks:**
- Create target array `Y` from the `Class` column
- Standardize features with `StandardScaler`
- Split data: 80% train / 20% test (`random_state=2`)
- Tune hyperparameters using `GridSearchCV` (cv=10) for four models:
  - **Logistic Regression** — C, penalty, solver
  - **Support Vector Machine (SVM)** — kernel, C, gamma
  - **Decision Tree** — criterion, splitter, max_depth, max_features, min_samples
  - **K-Nearest Neighbors (KNN)** — n_neighbors, algorithm, p
- Evaluate each model on the test set and plot confusion matrices
- Compare all models and identify the best performer

**Libraries:** `scikit-learn`, `pandas`, `numpy`, `matplotlib`, `seaborn`

---

## Results Summary

| Model                | Cross-Val Accuracy | Notes                          |
|---------------------|--------------------|--------------------------------|
| Logistic Regression | ~0.83              | Prone to false positives       |
| SVM                 | ~0.83              | Robust across kernel types     |
| Decision Tree       | ~0.83              | Interpretable, tunable depth   |
| KNN                 | ~0.83              | Simple baseline, competitive   |

> All models achieve similar accuracy on this dataset (~83% CV). The best model is selected based on test set score evaluated in Task 12.

---

## Getting Started

### Prerequisites
```bash
pip install pandas numpy matplotlib seaborn scikit-learn folium requests beautifulsoup4
```

### Run Order
Run the notebooks in the following sequence for the full pipeline:

```
1. webscraping_solved.ipynb
2. api_data_collection_solved.ipynb
3. eda_dataviz_solved.ipynb
4. folium_solved.ipynb
5. ml_prediction_solved.ipynb
```

### Clone and Launch
```bash
git clone https://github.com/<your-username>/spacex-falcon9-landing-prediction.git
cd spacex-falcon9-landing-prediction
jupyter notebook
```

---

## Data Sources

| Dataset | Source |
|--------|--------|
| Wikipedia launch history | [List of Falcon 9 and Falcon Heavy launches (2021 snapshot)](https://en.wikipedia.org/w/index.php?title=List_of_Falcon_9_and_Falcon_Heavy_launches&oldid=1027686922) |
| SpaceX REST API | [api.spacexdata.com/v4](https://api.spacexdata.com/v4/launches/past) |
| IBM Skills Network datasets | `dataset_part_2.csv`, `dataset_part_3.csv`, `spacex_launch_geo.csv` |

---


## Acknowledgements

Based on the IBM Data Science Professional Certificate Capstone Project by IBM Corporation / Skills Network.  
Labs authored by Yan Luo, Nayef Abou Tayoun, Joseph Santarcangelo, and Pratiksha Verma.

---

*© IBM Corporation. Project notebooks solved and compiled for educational purposes.*
