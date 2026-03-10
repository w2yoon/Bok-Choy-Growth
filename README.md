## Bok Choy Growth Prediction AI Competition

*_Participated in the [KIST-hosted AI competition](https://dacon.io/en/competitions/official/235961/) (2022.08.17 ~ 2022.09.19) on plant growth prediction  
and ranked 22nd out of 171 teams._


### Overview
Predict the daily leaf area growth rate of bok choy using minute-level environmental sensor data collected during the plant growth period.

---
### Approach
**Temporal Aggregation**

The raw dataset contains 1-minute interval environmental signals, which are highly noisy for modeling plant growth dynamics.
To capture biologically meaningful patterns, the data was aggregated into daily statistics, transforming minute-level sensor streams into stable daily features.

**Noise and Outlier Handling**

Environmental sensor readings often contain spikes and measurement noise.
An **IQR-based outlier filtering** strategy was applied to replace abnormal values with the median, ensuring robust feature distributions before modeling.

**Temporal Smoothing**

Plant growth responds to cumulative environmental exposure rather than instantaneous measurements.
To model this effect, **exponential weighted moving averages (EWM)** were applied to smooth temporal fluctuations and emphasize longer-term environmental trends.

**Feature Selection**

Correlation analysis between environmental variables and growth rate was used to identify **highly relevant features**, including:

* internal temperature / humidity
* heating and cooling load
* LED light intensity

This step reduced noise from irrelevant control signals.

**Model Choice**

An **XGBoost regression model** was used because:

* environmental sensor data is structured tabular data
* gradient boosting effectively captures nonlinear interactions between environmental variables
* it performs robustly with moderate dataset sizes

**Training Strategy**

A **5-fold cross-validation** scheme was used to improve generalization and reduce variance across different plant cases.

Predictions from each fold were **ensemble-averaged** for the final output.

---
### Key Insight

Plant growth is influenced by cumulative environmental exposure rather than minute-level fluctuations, making temporal smoothing and daily feature aggregation critical for reliable prediction.
