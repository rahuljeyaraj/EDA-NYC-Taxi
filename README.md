# NYC Yellow Taxi Operations — Exploratory Data Analysis (2023)

This project performs a comprehensive exploratory data analysis (EDA) on the NYC Yellow Taxi Trip Records for the full year 2023, sourced from the NYC Taxi and Limousine Commission (TLC). The goal is to uncover demand patterns, operational inefficiencies, and pricing insights to help optimise taxi fleet operations across New York City.

---

## Repository Contents

```
├── EDA_Assg_NYC_Taxi_Starter.ipynb   # Main analysis notebook
├── README.md                          # Project overview (this file)
```

---

## Dataset

- **Source:** [NYC TLC Trip Record Data](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)
- **Period:** January 2023 – December 2023
- **Format:** Parquet files (one per month, 12 files total)
- **Shapefile:** NYC Taxi Zone boundaries (`taxi_zones.shp`) for geographic analysis

> The full dataset contains millions of rows. A **5% stratified random sample** (by date and hour) was used, yielding approximately 250,000–300,000 rows for analysis.

---

## Project Structure

### 1. Data Preparation
- Load 12 monthly Parquet files from the TLC
- Stratified sampling: 5% of trips per hour per date, using `random_state=42`
- Combine all months into a single DataFrame and save for reuse

### 2. Data Cleaning
- Reset index and drop irrelevant columns (`store_and_fwd_flag`)
- Merge duplicate airport fee columns (`airport_fee` + `Airport_fee`)
- Handle missing values in `passenger_count`, `RatecodeID`, and `congestion_surcharge`
- Remove outliers: invalid passenger counts, impossible distances/fares, trips outside 2023
- Correct negative monetary values using `.abs()`
- Engineer temporal features: `pickup_hour`, `pickup_day`, `pickup_month`, `trip_duration`, `quarter`, `is_weekend`

### 3. Exploratory Data Analysis

#### General EDA
- Variable classification (categorical vs numerical)
- Hourly, daily, and monthly pickup distribution
- Monthly and quarterly revenue trends
- Fare vs distance correlation analysis
- Payment type distribution
- Geographic analysis using GeoPandas and taxi zone shapefiles

#### Detailed EDA
- Slowest routes by average speed (mph)
- Hourly trip volumes and busy hours (scaled to full population)
- Weekday vs weekend hourly demand comparison
- Top 10 pickup and dropoff zones
- Pickup-to-dropoff ratio by zone
- Night hour (11 PM – 5 AM) traffic zones
- Night vs daytime revenue share
- Fare per mile per passenger by group size
- Fare per mile by hour, day, and vendor
- Distance-tiered fare comparison by vendor
- Tip percentage analysis by distance, passenger count, and hour
- Passenger count patterns across hours, days, and zones
- Surcharge frequency and zone-level analysis

### 4. Conclusions
- Routing and dispatching recommendations
- Strategic cab positioning across zones and time periods
- Data-driven pricing strategy adjustments

---

## Requirements

Install dependencies via pip:

```bash
pip install pandas numpy matplotlib seaborn geopandas pyarrow
```

| Library | Purpose |
|---|---|
| `pandas` | Data loading, cleaning, and analysis |
| `numpy` | Numerical operations |
| `matplotlib` | Plotting and visualisation |
| `seaborn` | Statistical charts and heatmaps |
| `geopandas` | Geographic analysis and choropleth maps |
| `pyarrow` | Reading Parquet files |

---

## Getting Started

1. **Clone the repository**
```bash
git clone https://github.com/rahuljeyaraj/EDA-NYC-Taxi.git
cd EDA-NYC-Taxi
```

2. **Download the dataset**

Download the 2023 Yellow Taxi Trip Records (Parquet format) from the [TLC website](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page) and place them in a folder named `data/raw/`.

Also download the [Taxi Zone Shapefile](https://data.cityofnewyork.us/Transportation/NYC-Taxi-Zones/d3c5-ddgc) and place it in `data/taxi_zones/`.

3. **Run the notebook**
```bash
jupyter notebook EDA_Assg_NYC_Taxi_Starter.ipynb
```

> **Note:** Update the file paths inside the notebook to match your local directory structure before running.

---

## Key Findings

| Insight | Finding |
|---|---|
| Busiest hours | Evening rush (6–7 PM) and morning rush (8–9 AM) |
| Busiest days | Wednesday and Thursday |
| Busiest months | May and October |
| Top pickup zone | JFK Airport (~105,000 sampled trips) |
| Slowest route | Garment District loop (avg. 4.9 mph) |
| Dominant payment | Credit card (~81% of trips) |
| Peak revenue months | May and October (~$5.5M each) |
| Revenue by quarter | Q2 and Q4 lead at 26.8% each |
| Night revenue share | 12.1% (11 PM – 5 AM) |
| Highest tip % hour | 6–7 PM (~22% average tip) |

---

## Geographic Analysis

The notebook uses the NYC Taxi Zone shapefile to produce choropleth maps showing trip volume and passenger count distribution across all 263 taxi zones. Central Manhattan consistently dominates in trip volume, while outer boroughs show higher average passenger counts in isolated zones.

---

## Limitations

- **Cash tips not recorded:** All tip analysis applies only to credit card transactions (~81% of trips). Actual tipping rates are likely different across the full population.
- **Sampled data:** Results are based on a 5% sample. Absolute trip counts should be scaled by 20× for population-level estimates.
- **Vendor gap in fare/mile:** The large difference in fare per mile between vendors (Creative Mobile vs VeriFone) is a trip-composition effect, not a pricing difference — both operate under the same TLC rate structure.

---

## License

This project is for educational purposes. The dataset is publicly available from the NYC TLC under their open data policy.
