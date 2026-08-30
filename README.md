# 🌫️ The Atmospheric Pulse of Varanasi
### Spatio-temporal and Data Driven Analysis of Urban Air Quality (2025)

<img width="2148" height="883" alt="heatmap_pm25" src="https://github.com/user-attachments/assets/b34b7d8e-2e5a-42a1-bc00-e335dfebdb46" />

*PM2.5 Month × Hour Heatmap — Varanasi 2025. Dark red = highest pollution. The worst hours are November–December nights.*

---

> Kashi has always been defined by its fires — the pyres at Manikarnika, diesel autos before sunrise, coal stoves at the ghats. When we looked at this city through data, we assumed we already knew what we would find. We were wrong.

---

## 📌 Project Overview

This repository contains the complete data pipeline, analysis notebooks, and findings from our MSc dissertation submitted to the **Centre for Interdisciplinary Mathematical Sciences (CIMS), Banaras Hindu University, Varanasi** — May 2026.

Despite being one of the most polluted cities on the Indo-Gangetic Plain, **no comprehensive full-year, hourly, multi-station data study of Varanasi's air quality existed** in the literature. This project fills that gap — 35,040 hourly records, 4 CPCB monitoring stations, 7 pollutants, 5 meteorological parameters, every hour of 2025.

| | |
|---|---|
| **Institution** | Centre for Interdisciplinary Mathematical Sciences, BHU |
| **Authors** | Shikha Joshi · Tanvi Baheti |
| **Supervisor** | Prof. Arun Kaushik, Department of Statistics, BHU |
| **Data Source** | CPCB CAAQMS Portal — app.cpcbccr.gov.in |
| **Period** | January 1, 2025 — December 31, 2025 |
| **Total Records** | 35,040 hourly observations |

---

## 📁 Repository Structure

```
varanasi-pollution-eda/
│
├── data/
│   ├── raw/                        ← CPCB raw CSV files (see Data Access below)
│   └── processed/
│       └── master_clean.csv        ← Cleaned master dataset (all 4 stations merged)
│
├── notebooks/
│   ├── 01_data_cleaning.ipynb      ← Pipeline: raw CSVs → SQLite → master dataset
│   ├── 02_eda.ipynb                ← Diurnal, seasonal, distribution analysis
│   └── 03_advanced_analysis.ipynb  ← Spatial, AQI, health risk, meteorological, anomaly
│
├── outputs/
│   └── charts/                     ← All generated visualisations
│
├── README.md
└── requirements.txt
```

---

## 📊 Data

### Monitoring Stations

| Station | Location | Land Use Type | Latitude | Longitude |
|---|---|---|---|---|
| **Ardhali Bazar** | NW Varanasi | Dense commercial / wholesale market | 25.35°N | 82.97°E |
| **Bhelupur** | Central Varanasi | Urban residential + mixed commercial | 25.30°N | 82.99°E |
| **BHU IESD** | S Varanasi (Assi) | University campus / semi-green | 25.27°N | 82.99°E |
| **Maldahiya** | E Varanasi | Major traffic corridor / road junction | 25.32°N | 83.01°E |

### Parameters

| Type | Parameters |
|---|---|
| **Pollutants** | PM2.5 · PM10 · NO · SO₂ · CO · Ozone · Benzene |
| **Meteorological** | Temperature · Relative Humidity · Wind Speed · Solar Radiation · Rainfall |

### Data Access

Raw hourly CSV files can be downloaded directly from the **CPCB CAAQMS portal**:
🔗 [app.cpcbccr.gov.in](https://app.cpcbccr.gov.in)

Select station → parameter → date range (Jan 1 2025 – Dec 31 2025) → Download CSV.
Place downloaded files inside `data/raw/` before running `01_data_cleaning.ipynb`.

---

## ⚙️ Methodology Pipeline

```
  Raw CPCB CSVs (4 stations)
          │
          ▼
  Parameter-specific missing value treatment
  (Linear interpolation for high-variability pollutants,
   Hourly median imputation for large gaps)
          │
          ▼
  Sensor validation — NO₂ flagged as faulty
  (Std dev: 0.12 µg/m³ across 8,760 hours → excluded)
          │
          ▼
  SQLite Database → master_clean.csv
  (35,040 rows × 22 columns)
          │
          ▼
  AQI Computation from scratch
  (Official CPCB sub-index formula, 5 pollutants)
          │
          ▼
  Exploratory Data Analysis          Advanced Analysis
  ├── Descriptive statistics          ├── Spatial comparison
  ├── Frequency distributions         ├── AQI category distribution
  ├── Diurnal conditional means       ├── Health risk quantification
  ├── Seasonal box plots              ├── Pearson correlation matrix
  └── Month × Hour heatmaps          ├── Wind threshold analysis
                                      ├── Temperature inversion flagging
                                      ├── Monsoon wet deposition analysis
                                      └── 99th percentile anomaly detection
```

---

## 🔍 Key Findings

### 1. The Finding That Changed Everything

<img width="1784" height="1182" alt="aqi_dominant_pollutant" src="https://github.com/user-attachments/assets/15069386-80a3-4d20-8eb1-1b596b5ebad5" />


**PM10 — coarse road dust — not PM2.5, was the single biggest driver of Varanasi's AQI.**
Up to **5,000 hours a year** at every single station. Not combustion. Not vehicles. Dust.
The pollutant the entire national conversation overlooks was quietly running the numbers in this city, hour after hour.

---

### 2. Varanasi's Winter Fog Is a Pollution Trap

<img width="1485" height="731" alt="temperature_inversion" src="https://github.com/user-attachments/assets/17ccb784-e4da-4773-ad09-4831bf926f86" />


Temperature inversions — calm wind (< 0.5 m/s) + high humidity (> 85%) — raised PM2.5 by **27% above baseline** (33.3 vs 26.2 µg/m³). When the Ganga fog settles, the atmosphere stops mixing and holds everything the city exhales close to the ground.

---

### 3. The Most Dangerous Corner of the City

<img width="2085" height="733" alt="health_risk_hours_monthly" src="https://github.com/user-attachments/assets/5a4fe899-1378-4fab-bc18-f22c4ecdd3cd" />

**Ardhali Bazar** — a wholesale market, not an industrial zone — recorded **937 hours of dangerous PM2.5** in 2025. Nearly **1 in every 9 hours** of the year. Narrow lanes, trucks before dawn, complete absence of green cover — all of it visible in the numbers.

---

### 4. Green Cover Is Measurable Protection

<img width="2234" height="741" alt="station_ranking" src="https://github.com/user-attachments/assets/a9d8e3ff-f2b3-48a7-a745-2cefb374f648" />

**BHU's green campus** recorded **344 fewer dangerous hours** annually than Ardhali Bazar.
Same city. Same meteorology. The only difference is trees.

---

### 5. Monsoon Is Nature's Air Purifier

Monsoon rainfall (June–September) reduced PM2.5 by **20.7%** — from 27.8 µg/m³ on non-rainy hours to 22.0 µg/m³ on rainy hours. Health risk hours dropped to near zero across all stations during monsoon months.

---

### 6. Post-Monsoon, Not Winter, Was the Worst Season in 2025

<img width="2084" height="741" alt="rainfall_effect" src="https://github.com/user-attachments/assets/e2bb58f4-8860-42de-bc39-0056ef2bd180" />


Post-Monsoon (October–November) recorded the highest PM2.5 medians — peaking at **63 µg/m³ at 10pm** — driven by Diwali fireworks, monsoon withdrawal, and falling temperatures. The worst single day: **December 13 at Ardhali Bazar, AQI 240.4**.

---

## 🚀 How to Run

```bash
# 1. Clone the repository
git clone https://github.com/shikha-joshi-tech/varanasi-pollution-eda.git
cd varanasi-pollution-eda

# 2. Install dependencies
pip install -r requirements.txt

# 3. Download raw data from CPCB portal and place in data/raw/

# 4. Run notebooks in order
jupyter notebook notebooks/01_data_cleaning.ipynb
jupyter notebook notebooks/02_eda.ipynb
jupyter notebook notebooks/03_advanced_analysis.ipynb
```

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-white?style=for-the-badge&logo=matplotlib&logoColor=black)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)

| Library | Purpose |
|---|---|
| `pandas` | Data manipulation, cleaning, groupby operations |
| `numpy` | Numerical computations, percentile calculations |
| `matplotlib` | All chart generation |
| `seaborn` | Heatmaps, box plots, distribution plots |
| `sqlite3` | Structured data storage and SQL querying |

---

## 📋 Requirements

```
pandas>=1.5.0
numpy>=1.23.0
matplotlib>=3.6.0
seaborn>=0.12.0
jupyter>=1.0.0
```

---

## 📚 References

- Central Pollution Control Board (CPCB). (2014). *Air Quality Index — A Tool to Communicate Air Pollution Level.* Ministry of Environment, Forest and Climate Change, New Delhi.
- CPCB. (2009). *National Ambient Air Quality Standards (NAAQS).* Gazette of India.
- World Health Organization. (2021). *WHO Global Air Quality Guidelines.* Geneva.

---

## 👩‍💻 Authors

**Shikha Joshi**
MSc Mathematics & Computing · CIMS, BHU
[LinkedIn](https://linkedin.com/in/shikhajoshiii) · [GitHub](https://github.com/shikha-joshi-tech)

**Tanvi Baheti**
MSc Mathematics & Computing · CIMS, BHU

**Supervisor: Prof. Arun Kaushik**
Department of Statistics, Institute of Science, BHU

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

*"Data does not always confirm what a city's story leads you to believe. Sometimes it asks you to look at the ground beneath your feet."*
