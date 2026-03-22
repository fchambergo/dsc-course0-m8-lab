# Aviation Accident Analysis

A data analysis project for a consulting firm tasked with identifying safe aircraft makes and models for commercial airline operations.

## Project Overview

This analysis examines aviation accident data from 1948-2023 to recommend aircraft that exhibit:
- **Low rates of total destruction** in accidents
- **Low likelihood of fatal or serious passenger injuries** in accidents
- **Statistical robustness** with sufficient historical data (40+ years, professionally built models only)

The analysis separates small aircraft (< 20 passengers) from larger passenger models to provide distinct recommendations for each category.

## Data Source

- **File**: `data/AviationData.csv`
- **Records**: Full dataset of aviation accidents spanning 1948-2023
- **Key Variables**: Aircraft make/model, damage severity, injury counts, weather, engine type, phase of flight, and more

## Project Structure

### 1. Aviation_Accidents_Cleaning.ipynb

**Purpose**: Data preparation and feature engineering

**Tasks**:
- Load raw aviation data and inspect for missing values and data types
- Filter to professional aircraft in the 1983-2023 window (models within max 40-year lifetime)
- Remove amateur builds and non-airplane incidents
- Clean and standardize key columns (Make, Model, Aircraft.damage, Weather, Engine.Type, etc.)
- Create derived safety metrics:
  - `serious_fatal_injuries`: Count of fatal + serious injuries per accident
  - `total_people_onboard_est`: Estimated passengers (sum of all injury outcomes)
  - `serious_fatal_rate`: Fraction of passengers with serious/fatal injuries
  - `was_destroyed`: Binary indicator of total aircraft destruction
  - `Make_Model`: Unique aircraft type identifier
- Apply sample-size thresholds (≥50 records per Make for robustness)
- Remove columns with >80% missing values
- **Output**: `data/AviationData_cleaned.csv` for downstream analysis

**Run this notebook first** to generate the cleaned dataset.

### 2. Aviation_Accidents_Data_Analysis.ipynb

**Purpose**: Exploratory data analysis and safety recommendations

**Tasks**:
- Load cleaned data and separate into small vs. large aircraft (threshold: 20 passengers)
- Analyze injury rates by aircraft manufacturer (Make):
  - Identify 15 lowest-risk makes in each category
  - Visualize with bar charts and distribution plots
  - Compare small and large aircraft side-by-side
- Analyze aircraft destruction rates by Make
  - Rank by lowest destruction rates
  - Select top 5 safe recommendations combining injury + destruction metrics
- Model-level analysis for larger aircraft:
  - Identify specific make/model combinations with low injury rates
  - Filter for models with ≥10 historical accidents for stability
- Explore two key safety factors:
  - **Weather Condition**: Compare injury and destruction rates across weather types
  - **Broad Phase of Flight**: Compare outcomes across takeoff, cruise, landing phases
- **Output**: Summary statistics, visualizations, and final safety recommendations

**Run this notebook second** after the cleaned data is generated.

## How to Use

### Prerequisites
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### Workflow

1. **Run Cleaning Notebook First**:
   ```
   jupyter notebook Aviation_Accidents_Cleaning.ipynb
   ```
   - Executes all cells to load, clean, and save `data/AviationData_cleaned.csv`

2. **Run Analysis Notebook Second**:
   ```
   jupyter notebook Aviation_Accidents_Data_Analysis.ipynb
   ```
   - Loads the cleaned CSV and performs all exploratory analysis
   - Generates visualizations and safety recommendations

### Key Outputs

#### From Cleaning:
- Clean dataset with engineered safety metrics
- Standardized categorical variables
- Sample-size filtered make/model selections

#### From Analysis:
- **Make Recommendations**:
  - Top 5 small aircraft manufacturers by safety profile
  - Top 5 large aircraft manufacturers by safety profile
- **Model Recommendations**: 
  - Specific large aircraft models with lowest injury rates
- **Safety Factor Insights**:
  - How weather conditions affect accident severity
  - How flight phase correlates with injuries and destruction
- **Visualizations**:
  - Bar charts: Mean injury rates by make
  - Violin/strip plots: Injury rate distributions
  - Comparative weather and flight-phase safety profiles

## Key Assumptions & Notes

- **Missing Data**: Injury counts are filled with 0 when missing; onboard estimates use all injury outcomes
- **Sample Thresholds**: Makes require ≥50 records; models require ≥10 records for inclusion
- **Injury Rate Bounds**: All rates clipped to [0, 1] to handle data entry artifacts
- **Passenger Estimate**: Derived from summing all injury categories (not always explicit in data)
- **Time Window**: 1983 onward only (40-year cutoff for active models)

## Recommendations to Client

The analysis identifies aircraft makes and specific models exhibiting:
1. Historically low serious/fatal injury rates relative to accidents
2. Low destruction rates, indicating airframe robustness
3. Sufficient operational history (≥50 accidents, ≥10 models) for confident assessment

Final recommendations should also factor in availability, cost, maintenance infrastructure, and current operational status of recommended aircraft.

## Files

```
.
├── README.md                                    # This file
├── Aviation_Accidents_Cleaning.ipynb            # Data cleaning & preparation
├── Aviation_Accidents_Data_Analysis.ipynb       # EDA & recommendations
├── data/
│   ├── AviationData.csv                         # Raw aviation accident dataset
│   ├── AviationData_cleaned.csv                 # Cleaned, engineered dataset
│   └── USState_Codes.csv                        # Reference lookup (if used)
└── LICENSE                                      # Project license
```

