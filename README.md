<div align="center">

# 🎓 EduVision Dashboard

**An interactive Tableau dashboard for exploring and comparing global universities**

Combining the *Times Higher Education (THE) World University Rankings 2016–2026* with the *QS World University Rankings 2025*

![Tableau](https://img.shields.io/badge/Tableau-E97627?style=for-the-badge&logo=tableau&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![Plotly](https://img.shields.io/badge/Plotly-3F4F75?style=for-the-badge&logo=plotly&logoColor=white)

</div>

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Dashboard Preview](#-dashboard-preview)
- [Key Features](#-key-features)
- [Repository Structure](#-repository-structure)
- [Data Sources](#-data-sources)
- [Data Pipeline](#-data-pipeline)
- [Engineered Metrics](#-engineered-metrics)
- [Getting Started](#-getting-started)
- [Tech Stack](#-tech-stack)
- [Limitations & Future Work](#-limitations--future-work)

---

## 🔍 Overview

University rankings are published by several organisations, each using a different methodology, which makes direct comparison difficult. **EduVision** brings two major ranking systems into a single, cleaned dataset and presents the results in an interactive dashboard.

The project covers the full analytics workflow: **data cleaning and merging in Python**, **feature engineering** (a custom *Performance Index*), and **visual storytelling in Tableau**.

With EduVision you can:

- Compare institutions across countries and ranking systems
- Explore trends in research quality, student population and internationalisation
- Identify top performers using a composite Performance Index

---

## 🖼️ Dashboard Preview

### Overview
<img width="800" alt="Overview" src="https://github.com/user-attachments/assets/99c3dc29-687d-4418-8d48-75f23111efdd" />

### Research
<img width="800" alt="Research" src="https://github.com/user-attachments/assets/c012a7b2-00ec-48c7-993a-e5832cbd9930" />

### Students
<img width="800" alt="Students" src="https://github.com/user-attachments/assets/e90bf7e8-f58b-4070-9257-d8a8566bc5fe" />

### Country
<img width="800" alt="Country" src="https://github.com/user-attachments/assets/b34a7506-a9c4-491a-a2cf-9ff3ffd4a063" />

---

## ✨ Key Features

| | Feature | Description |
|---|---|---|
| 🔗 | **Merged dataset** | Combines QS 2025 and THE 2016–2026 rankings |
| 🧹 | **Automated cleaning** | Handles messy fields such as ranges (`"20,000-25,000"`), percentages and `n/a` values |
| 📊 | **Performance Index** | Composite score blending rank, research productivity and international outreach |
| 🌍 | **Country summary** | Country-level table for geographic comparison |
| 🖥️ | **Interactive workbook** | Tableau `.twbx` file with the data packaged inside |

---

## 📂 Repository Structure

```text
EduVision_Dashboard/
├── EduVision Dashboard.twbx                     # Tableau packaged workbook (final dashboard)
├── edvision_data_cleaning.py                    # Cleaning, merging & feature engineering script
├── qs-world-rankings-2025.csv                   # Raw QS World University Rankings 2025
├── THE World University Rankings 2016-2026.csv  # Raw THE World University Rankings 2016–2026
├── final_merged_university_rankings.csv         # Merged QS + THE dataset
├── university_final_dataset.csv                 # Final cleaned dataset with engineered features
├── country_summary.xlsx                         # Aggregated statistics by country
└── README.md
```

---

## 📚 Data Sources

| Dataset | Description |
|---|---|
| **QS World University Rankings 2025** | Institution rankings and scores from Quacquarelli Symonds |
| **THE World University Rankings 2016–2026** | Times Higher Education rankings with research quality, student population, staff ratios and international student data |

> [!NOTE]
> Rankings data belongs to its respective publishers (QS and Times Higher Education). It is used here for educational and analytical purposes only.

---

## ⚙️ Data Pipeline

The cleaning logic lives in [`edvision_data_cleaning.py`](edvision_data_cleaning.py) (originally developed in Google Colab).

1. **Load** the QS and THE CSV files with `pandas`
2. **Remove duplicates** from both datasets
3. **Standardise text**: institution names and country/location fields are stripped and lower-cased to enable matching
4. **Merge** QS and THE on institution name + country (outer join, so no institution is lost)
5. **Clean numeric fields**
   - `Student Population`: ranges (`"20,000-25,000"`), `+` and `~` values converted to numbers
   - `International Students`: percentage strings and ranges converted to numeric values
   - `n/a` values converted to nulls
6. **Engineer features** (see below)
7. **Normalise** selected metrics to a 0–1 scale using `MinMaxScaler`
8. **Export** the final datasets used by Tableau

---

## 📐 Engineered Metrics

| Metric | Formula | Purpose |
|---|---|---|
| `Global_Rank_Score` | `100 − Rank` (THE rank) | Converts rank so that higher = better |
| `Research_Productivity_Index` | `Research Quality ÷ Students to Staff Ratio` | Research output relative to teaching load |
| `Intl_Student_Percentage` | Cleaned international student % | Measures global outreach |
| **`Performance_Index`** | `0.4 × Global_Rank_Score + 0.3 × Research_Productivity_Index + 0.3 × Intl_Student_Percentage` | Composite score (inputs min-max normalised) |

The weights (40 / 30 / 30) are a design choice and can be adjusted in the script to reflect different priorities.

---

## 🚀 Getting Started

### Prerequisites

- [Tableau Desktop](https://www.tableau.com/products/desktop) or [Tableau Public](https://public.tableau.com/) (free) to open the dashboard
- Python 3.8+ (only needed to re-run the data cleaning)

### View the dashboard

1. Clone the repository:

   ```bash
   git clone https://github.com/amrutham004/EduVision_Dashboard.git
   cd EduVision_Dashboard
   ```

2. Open `EduVision Dashboard.twbx` in Tableau Desktop or Tableau Public. The data is packaged inside the workbook, so no extra setup is needed.

### Re-run the data cleaning (optional)

```bash
pip install pandas scikit-learn plotly
python edvision_data_cleaning.py
```

> [!IMPORTANT]
> The script was written for Google Colab and mounts Google Drive (`from google.colab import drive`). To run it locally, remove the Drive-mounting lines and change the file paths to point to the CSV files in this repository.

---

## 🧰 Tech Stack

| Tool | Used for |
|---|---|
| **Tableau** | Interactive dashboard and visualisation |
| **Python** (`pandas`, `scikit-learn`, `plotly`) | Cleaning, feature engineering and exploration |
| **Google Colab** | Data cleaning environment |
| **Excel / CSV** | Data storage and summaries |

---

## 🔭 Limitations & Future Work

**Limitations**

- Institution matching uses exact name + country text matching, so differently spelled names across QS and THE may not merge. Fuzzy matching could improve coverage.
- The Performance Index uses fixed weights and the THE rank only; it is a simple composite, not an official ranking.

**Planned improvements**

- [ ] Add more ranking sources
- [ ] Include year-over-year trend analysis
- [ ] Publish the dashboard on Tableau Public

---

<div align="center">

Made by [@amrutham004](https://github.com/amrutham004) · ⭐ Star this repo if you found it useful!

</div>
