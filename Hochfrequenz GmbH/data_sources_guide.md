# Data Sources & Real-World References

## Recommended Datasets for Workshop

### Primary Dataset: German Household Smart Meters (Recommended ✅)

**Citation:** Schlemminger, M., et al. (2022). Dataset on electrical single-family house and heat pump load profiles. Nature Scientific Data, 9, 265.

**Source:** Zenodo  
**URL:** https://zenodo.org/records/5642902
**Size:** ~2.8 GB (raw), 15.1 GB (uncompressed)  
**Coverage:** 38 households, Northern Germany, May 2018 – Dec 2020  
**Temporal Resolution:** 10s, 1min, 15min, 60min aggregations  
**License:** CC-BY-4.0 (Creative Commons)

**Key Metrics:**
- Apparent power (W)
- Active power (W)
- Reactive power (VAR)
- Voltage (V)
- Current (A)
- Power factor

**Why it's perfect:**
- Real German data (directly relevant)
- Smart meter granularity (exactly what Hochfrequenz clients use)
- 2+ years captures seasonality
- Public & reproducible
- High quality (90%+ availability)

**Workshop Usage:**
```python
import h5py

# Load from HDF5 format
with h5py.File('data.hdf5', 'r') as f:
    # Browse structure
    for household in f.keys():
        consumption = f[household]['electricity']['apparent_power'][:]  # NumPy array
```

---

### Alternative Dataset: Spanish Household Consumption

**Citation:** Quesada, C., et al. (2024). An electricity smart meter dataset of Spanish households. Nature Scientific Data, 11, 96.

**Source:** Zenodo  
**URL:** https://zenodo.org/record/10583937  
**Coverage:** 25,559 Spanish households  
**Data Format:** CSV (easier to load)  
**Temporal Resolution:** Hourly  

**Advantages:**
- Much larger dataset (good for deep learning)
- Easier format (CSV vs HDF5)
- Broader geographic variation

**Disadvantages:**
- Not German (less relevant for workshop context)
- Only one year of data

---

### US Data: UCI ML Repository

**Dataset:** Electricity Consuming Load Study (ECS)  
**URL:** https://archive.ics.uci.edu/ml/datasets/electricity+consuming+load  

**Details:**
- 10-minute resolution electricity load
- Multiple buildings
- 1+ year of data

**Use case:** Show international comparison

---

## Synthetic Data Generation

If you prefer **not** to download real data, the workshop includes a **synthetic data generator** in `01_regression_forecasting.ipynb` that creates realistic consumption patterns:

```python
# Features:
# - Realistic hourly patterns (peaks 7am, 6pm)
# - Seasonal variation (winter higher)
# - Weekend effect (10-15% reduction)
# - Weather correlation (temperature)
# - Random noise

# Output: ~18,000 hourly samples (2 years)
# File size: <5 MB in CSV format
```

**Pros:**
- No download required
- Deterministic (reproducible)
- Scalable (generate more data on demand)

**Cons:**
- Less realistic variability
- Doesn't capture true consumption anomalies
- Participants might think "is this too simple?"

**Recommendation:** Use synthetic for first workshop (faster), upgrade to real data later.

---

## How to Customize Data for Different Audiences

### Scenario 1: DSO (Distribution System Operator)

**Focus:** Aggregate household consumption across district

```python
# Instead of one household, aggregate 500 households
# Shows grid-level patterns:
# - More stable (law of large numbers)
# - Clear seasonal peaks
# - Renewable integration effects visible

# Modify notebooks:
# - Aggregate multiple household signals
# - Higher peak values (~500 MW vs 500 W)
# - Add solar/wind integration noise
```

### Scenario 2: Industrial Customer

**Focus:** Heavy manufacturing load patterns

```python
# Replace consumption with industrial profile:
# - Multi-shift operation (24/7 if possible)
# - Much higher magnitude (MW range)
# - Equipment on/off patterns
# - Maintenance shutdown effects

data['consumption'] = industrial_baseline + equipment_loads + noise
```

### Scenario 3: Renewable Energy Forecasting

**Focus:** Solar/wind generation patterns

```python
# Replace consumption with solar generation:
# - Clear daily cycle (0 at night, peak noon)
# - Seasonal variation (latitude-dependent)
# - Weather effects (clouds → dips)
# - High variability (volatility characteristic)

data['consumption'] = solar_irradiance * panel_efficiency + noise
```

---

## Data Augmentation Ideas

### Add Weather Data

```python
# Correlation: Temperature affects consumption
# Heating (winter), cooling (summer)

# Weather API options:
# - NOAA (US)
# - DWD (German Meteorological Service)
# - OpenWeatherMap (free tier limited)

weather = pd.read_csv('weather_data.csv')
data = data.merge(weather, on='timestamp')
data['heating_degree_days'] = (65 - data['temperature']).clip(lower=0)
```

### Add Calendar Effects

```python
import holidays

# German holidays affect consumption patterns
de_holidays = holidays.DE()

data['is_holiday'] = data['timestamp'].dt.date.isin(de_holidays)
data['days_to_holiday'] = data['timestamp'].apply(
    lambda x: min([abs((x.date() - h).days) 
                   for h in de_holidays.keys()])
)
```

### Add Market Data

```python
# Electricity price correlates with demand
# Energy trading relevant for market communication

market_data = pd.read_csv('electricity_prices.csv')
data = data.merge(market_data, on='timestamp')
# Incorporate into anomaly detection (unusual price + consumption)
```

---

## Data Quality Checks

Before using any dataset, validate:

```python
import pandas as pd

def validate_energy_data(df):
    """Check data quality for energy consumption data."""
    
    checks = {}
    
    # 1. Check for duplicates
    checks['duplicates'] = len(df) - len(df.drop_duplicates())
    
    # 2. Check for negative values (impossible in consumption)
    checks['negative_values'] = (df['consumption'] < 0).sum()
    
    # 3. Check for missing values
    checks['missing_percent'] = df.isnull().sum() / len(df) * 100
    
    # 4. Check time continuity
    time_diffs = df.index.to_series().diff().value_counts()
    checks['expected_frequency'] = time_diffs.index[0]  # Most common
    
    # 5. Check outliers (>3 std)
    mean = df['consumption'].mean()
    std = df['consumption'].std()
    checks['outliers'] = ((df['consumption'] > mean + 3*std) | 
                          (df['consumption'] < mean - 3*std)).sum()
    
    # 6. Consumption statistics
    checks['mean_w'] = df['consumption'].mean()
    checks['std_w'] = df['consumption'].std()
    checks['min_w'] = df['consumption'].min()
    checks['max_w'] = df['consumption'].max()
    
    return pd.Series(checks)

# Run validation
validation = validate_energy_data(data)
print(validation)
```

---

## Production Data Integration

If workshop leads to actual implementation:

### Real Data Pipeline

```
Smart Meters → Data Collectors → Cloud Storage → ETL Pipeline → ML Models → Predictions
         (kWh/hour)      (IoT hubs)    (S3/Blob)   (Apache Spark)  (TensorFlow)  (API)
```

### Hochfrequenz-Specific Considerations

**Integration Points:**

1. **SAP IS-U Data**
   - Billings system contains historical consumption
   - Extract via ABAP queries or APIs
   - Usually hourly or daily granularity

2. **Market Communication (MaKo 2020)**
   - Standardized data format
   - EDIFACT or XML
   - 15-minute metering resolution

3. **Smart Meter Gateways (SMGw)**
   - MBus protocol
   - Secure data transmission
   - 15-60 minute aggregation

**Data Governance:**
- GDPR compliance (customer anonymization)
- Data retention policies
- Access controls
- Audit logging

---

## Troubleshooting Data Issues

### Problem: Dataset Too Large for Memory

```python
# Solution 1: Resample to lower resolution
data_hourly = data_15min.resample('H').mean()

# Solution 2: Load in chunks
chunks = pd.read_csv('data.csv', chunksize=100000)
for chunk in chunks:
    process_chunk(chunk)

# Solution 3: Use Dask for distributed processing
import dask.dataframe as dd
data_dask = dd.read_csv('data*.csv')
```

### Problem: Missing Values in Time Series

```python
# Don't drop rows! Interpolate to maintain sequence

# Solution 1: Forward fill (propagate last value)
data['consumption'] = data['consumption'].fillna(method='ffill')

# Solution 2: Linear interpolation
data['consumption'] = data['consumption'].interpolate(method='linear')

# Solution 3: Spline interpolation (smoother)
data['consumption'] = data['consumption'].interpolate(method='spline', order=2)
```

### Problem: Extreme Outliers

```python
# Winsorizing: Cap extreme values

from scipy.stats import mstats

data['consumption_winsorized'] = mstats.winsorize(
    data['consumption'], 
    limits=[0.01, 0.01]  # Cap top/bottom 1%
)
```

---

## Dataset License Compliance

### German Smart Meter Data

**License:** CC-BY-4.0  
**Requirements:**
- ✅ Attribution required
- ✅ Commercial use allowed
- ✅ Modifications allowed
- ✅ Distribution allowed

**Citation requirement:**
```
Schlemminger, M., Ramasamy, A., & Blume, S. (2022). 
Dataset on electrical single-family house and heat pump load profiles. 
Nature Scientific Data, 9(1), 265. 
https://doi.org/10.1038/s41597-022-01156-1
```

### Workshop Attribution

Include in materials:
```
Data Source: Schlemminger et al., Nature Scientific Data (2022)
Dataset: https://zenodo.org/record/6445482
License: Creative Commons Attribution 4.0 International
```

---

## References for Further Reading

### Energy Consumption Forecasting

1. **Survey:** Fallah, S. N., et al. (2020). "Short-term electricity load forecasting for residential customers: A machine learning approach." Energy, 194, 116889.

2. **LSTM for Energy:** Kim, T. Y., & Cho, S. B. (2019). "Predicting residential energy consumption using CNN-LSTM neural networks." Energy, 182, 72-81.

3. **Anomaly Detection:** Hubbard, M., et al. (2013). "Smart meter data analytics for customer segmentation." SMLR, 1-10.

### Time Series Deep Learning

- Goodfellow, I., Bengio, Y., & Courville, A. (2016). *Deep Learning*. MIT Press. [Ch. 10: Sequence Modeling]
- Chollet, F. (2021). *Deep Learning with Python*. Manning. [Ch. 6: Time Series]

### Energy Sector Context

- European Commission. (2020). "Market design in European electricity markets." ACER Report.
- German Bundesnetzagentur. (2024). "Smart Meter Rollout Status."

---

## Quick Data Reference

| Dataset | Size | Households | Period | Resolution | Format | License |
|---------|------|-----------|--------|-------------|--------|---------|
| **German (Schlemminger)** | 2.8 GB | 38 | 2018-2020 | 10s-60m | HDF5 | CC-BY |
| **Spanish (Quesada)** | Large | 25,559 | 1 year | Hourly | CSV | CC-BY |
| **Synthetic (Workshop)** | <5 MB | 1 | 2 years | Hourly | CSV | N/A |

---

**Last updated:** December 5, 2024  
**For data questions:** Reach out to workshop coordinator
