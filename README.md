EduVision Dashboard

An interactive Tableau dashboard for exploring and comparing global universities, built by combining the Times Higher Education (THE) World University Rankings (2016–2026) with the QS World University Rankings 2025.

The project covers the full analytics workflow: data cleaning and merging in Python, feature engineering (a custom Performance Index), and visual storytelling in Table. 

Table of Contents
Overview
Key Features
Repository Structure
Data Sources
Data Pipeline
Engineered Metrics
Getting Started
Dashboard Preview
Tech Stack
Limitations & Future Work


Overview
University rankings are published by several organisations, each using different methodologies, which makes comparison difficult. EduVision brings two major ranking systems into a single, cleaned dataset and presents the results in an interactive dashboard so users can:
Compare institutions across countries and ranking systems
Explore trends in research quality, student population and internationalisation
Identify top performers using a composite Performance Index

Key Features
🔗 Merged dataset combining QS 2025 and THE 2016–2026 rankings
🧹 Automated cleaning of messy fields (ranges such as "20,000-25,000", percentages, n/a values)
📊 Composite Performance Index that blends rank, research productivity and international outreach
🌍 Country-level summary table for geographic comparison
🖥️ Interactive Tableau workbook (.twbx) with the data packaged inside

Repository Structure
EduVision_Dashboard/
├── EduVision Dashboard.twbx                    # Tableau packaged workbook (final dashboard)
├── edvision_data_cleaning.py                   # Data cleaning, merging & feature engineering script
├── qs-world-rankings-2025.csv                  # Raw QS World University Rankings 2025
├── THE World University Rankings 2016-2026.csv # Raw THE World University Rankings 2016–2026
├── final_merged_university_rankings.csv        # Merged QS + THE dataset
├── university_final_dataset.csv                # Final cleaned dataset with engineered features
├── country_summary.xlsx                        # Aggregated statistics by country
└── README.md

Data Sources
Dataset	Description
QS World University Rankings 2025	Institution rankings and scores from Quacquarelli Symonds
THE World University Rankings 2016–2026	Times Higher Education rankings with research quality, student population, staff ratios and international student data

Note: Rankings data belongs to its respective publishers (QS and Times Higher Education). It is used here for educational and analytical purposes only.

Data Pipeline
The cleaning logic lives in edvision_data_cleaning.py (originally developed in Google Colab).
Load the QS and THE CSV files with pandas
Remove duplicates from both datasets
Standardise text – institution names and country/location fields are stripped and lower-cased to enable matching
Merge QS and THE on institution name + country (outer join, so no institution is lost)
Clean numeric fields
Student Population → ranges ("20,000-25,000"), + and ~ values converted to numbers
International Students → percentage strings and ranges converted to numeric values
n/a values converted to nulls
Engineer features (see below)
Normalise selected metrics to a 0–1 scale using MinMaxScaler
Export the final datasets used by Tableau
Engineered Metrics
Metric	Formula	Purpose
Global_Rank_Score	100 − Rank (THE rank)	Converts rank so that higher = better
Research_Productivity_Index	Research Quality ÷ Students to Staff Ratio	Research output relative to teaching load
Intl_Student_Percentage	Cleaned international student %	Measures global outreach
Performance_Index	0.4 × Global_Rank_Score + 0.3 × Research_Productivity_Index + 0.3 × Intl_Student_Percentage	Composite score (inputs min-max normalised)
The weights (40 / 30 / 30) are a design choice and can be adjusted in the script to reflect different priorities.

Getting Started
Prerequisites
Tableau Desktop or Tableau Public (free) to open the dashboard
Python 3.8+ (only needed to re-run the data cleaning)
View the dashboard
Clone the repository
bash
   git clone https://github.com/amrutham004/EduVision_Dashboard.git
   cd EduVision_Dashboard
Open EduVision Dashboard.twbx in Tableau Desktop or Tableau Public. The data is packaged inside the workbook, so no extra setup is needed.


The script was written for Google Colab and mounts Google Drive (from google.colab import drive). To run it locally, remove the Drive-mounting lines and change the file paths to point to the CSV files in this repository.

Dashboard Preview
<img width="1600" height="865" alt="Overview" src="https://github.com/user-attachments/assets/99c3dc29-687d-4418-8d48-75f23111efdd" />
<img width="1600" height="869" alt="research" src="https://github.com/user-attachments/assets/c012a7b2-00ec-48c7-993a-e5832cbd9930" />
<img width="1600" height="791" alt="student" src="https://github.com/user-attachments/assets/e90bf7e8-f58b-4070-9257-d8a8566bc5fe" />
<img width="1600" height="789" alt="country" src="https://github.com/user-attachments/assets/b34a7506-a9c4-491a-a2cf-9ff3ffd4a063" />

Tech Stack
Tableau – interactive dashboard and visualisation
Python – pandas, scikit-learn, plotly
Google Colab – data cleaning environment
Excel / CSV – data storage and summaries

Limitations & Future Work
Institution matching uses exact name + country text matching, so differently spelled names across QS and THE may not merge. Fuzzy matching could improve coverage.
The Performance Index uses fixed weights and the THE rank only; it is a simple composite, not an official ranking.
Planned improvements: add more ranking sources, include year-over-year trend analysis, and publish the dashboard on Tableau Public.
