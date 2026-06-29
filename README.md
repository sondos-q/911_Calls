# 911 Calls — Exploratory Data Analysis

An exploratory data analysis (EDA) project on 911 emergency call data from Montgomery County, Pennsylvania, sourced from Kaggle. The project uncovers call patterns by reason, time of day, day of week, and month using Python visualizations.

---

## Dataset

| Property | Value |
|---|---|
| Source file | `911.csv` |
| Total records | 99,492 |
| Time period | 2015–2016 |

### Columns

| Column | Type | Description |
|---|---|---|
| `lat` | float | Latitude |
| `lng` | float | Longitude |
| `desc` | string | Description of the emergency call |
| `zip` | float | Zipcode |
| `title` | string | Title/code of the call (e.g. `EMS: BACK PAINS/INJURY`) |
| `timeStamp` | string → datetime | Date and time of the call (`YYYY-MM-DD HH:MM:SS`) |
| `twp` | string | Township |
| `addr` | string | Street address |
| `e` | int | Dummy variable (always 1) |

---

## Project Structure

```
├── 911.csv                  # Raw data
├── 911_calls_eda.ipynb      # Full analysis notebook
└── README.md
```

---

## Setup

### Requirements

```bash
pip install pandas numpy matplotlib seaborn
```

### Run

Open and run `911_calls_eda.ipynb` in Jupyter Notebook or JupyterLab.

---

## Analysis Walkthrough

### 1. Data Cleaning
- Missing values in `zip`, `twp`, and `addr` were filled using backward/forward fill (`bfill`/`ffill`)
- No duplicate rows were found

### 2. Feature Engineering

New columns derived from existing data:

| New Column | Source | Description |
|---|---|---|
| `Reason` | `title` | Emergency category extracted from title prefix (`EMS`, `Fire`, `Traffic`) |
| `Hour` | `timeStamp` | Hour of the call (0–23) |
| `Month` | `timeStamp` | Month of the call (1–12) |
| `Day of Week` | `timeStamp` | Day name (`Mon`–`Sun`) |
| `Date` | `timeStamp` | Calendar date (date only, no time) |

### 3. Key Findings

**Top 5 Zipcodes by call volume:**
| Zipcode | Calls |
|---|---|
| 19401 | 7,866 |
| 19464 | 7,605 |
| 19403 | 5,596 |
| 19446 | 5,539 |
| 19406 | 3,636 |

**Top 5 Townships by call volume:**
| Township | Calls |
|---|---|
| Lower Merion | 8,444 |
| Abington | 5,986 |
| Norristown | 5,895 |
| Upper Merion | 5,231 |
| Cheltenham | 4,576 |

**Calls by Reason:**
| Reason | Calls |
|---|---|
| EMS | 48,877 |
| Traffic | 35,695 |
| Fire | 14,920 |

EMS is the most common reason, accounting for nearly half of all calls.

---

## Visualizations

| Plot | Description |
|---|---|
| Countplot by Reason | Distribution of calls across EMS, Fire, and Traffic |
| Countplot by Day of Week | Call volume per day, broken down by Reason |
| Countplot by Month | Call volume per month, broken down by Reason |
| Line plot — calls per month | Trend of total calls over available months |
| Linear fit (seaborn lmplot) | Regression line over monthly call counts |
| Time series by Date | Daily call volume overall and split by Reason |
| Heatmap — Day of Week × Hour | Call intensity by hour and weekday |
| Clustermap — Day of Week × Hour | Clustered version of the hourly heatmap |
| Heatmap — Day of Week × Month | Call intensity by month and weekday |
| Clustermap — Day of Week × Month | Clustered version of the monthly heatmap |

---

## Notable Observations

- **EMS dominates** all days and times, consistently the most frequent call type
- **Weekday peaks** occur during morning commute hours (~8–9 AM) and afternoon (~5 PM), likely driven by Traffic calls
- **Weekends** show a flatter hourly curve with more evenly distributed call times
- **Some months are missing** from the dataset (September–November), indicating incomplete data for that period
- The linear fit on monthly counts suggests a slight downward trend, though this is influenced by incomplete months
