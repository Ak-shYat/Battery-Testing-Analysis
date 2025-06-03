## 📖 Overview

This repository delivers a **structured and data-driven analysis** of lithium-ion battery performance over extensive time-series data. Key features include:

- **Visual Cues**: Stunning visualizations of voltage, capacity, and C-rate trends.
- **Practical Metrics**: State of Health (SOH), C-rate, and capacity fade analysis.
- **Statistical Rigor**: Correlation matrices and Pearson tests for robust insights.
- **Predictive Modeling**: Polynomial regression to forecast battery degradation.


## Project Workflow

### 1. Preliminary Data Preparation 
- **Action**: Loaded two large datasets (`LR1865SZ_cycles201214_002_4.xlsx`, `LR1865SZ_cycles201217_001_2.xlsx`) in Google Colab, stored in Google Drive.
- **Purpose**: Ensured comprehensive battery performance analysis.
- **Tools**: Verified Excel compatibility with Pandas; inspected data with `df.head()`.

### 2. Merging Datasets 
- **Action**: Vertically merged datasets using `pd.concat`, dropped `Data_Point` column due to inconsistent numbering.
- **Purpose**: Created a unified dataset without duplicate indices for seamless analysis.

### 3. Variable Analysis 
- **Action**: Used `df.head()`, `df.info()`, and `df.describe()` to explore data types, statistics, and missing values (`df.isnull().sum()`).
- **Purpose**: Validated dataset integrity, ensuring no missing values for reliable results.

### 4. Exploratory Data Analysis (EDA) 
- **Univariate**: Histograms for Voltage, Current, Capacity, and Energy to understand distributions.
- **Bivariate**: Scatter plots (e.g., Voltage vs. Current, Capacity vs. Voltage) to reveal interactions.
- **Cycle-Based**: Grouped by `Cycle_Index` to plot average Voltage and Capacity, showing trends like capacity fade.
- **Setup**: 3x3 subplot grid with Matplotlib for clear visualizations.

### 5. Data Cleaning (Time Conversion) 
- **Issue**: `Test_Time(s)` had string formats (e.g., 'H:MM:SS', 'D-H:MM:SS').
- **Action**: Converted to seconds using a custom `time_string_to_seconds` function.
- **Purpose**: Enabled accurate temporal calculations.

### 6. Charging and Discharging Capacity Calculation 
- **Separation**: Split data into charging (`Current(A) > 0`) and discharging (`Current(A) < 0`) phases.
- **Calculation**: Used `calculate_cycle_capacity` with formula:
Capacity(k+1) = Capacity(k) + |Current(k+1)| * (time(k+1) - time(k)) / 3600

- **Challenge**: Integral method caused capacities to exceed 50x reference capacity (2.3 Ah). Switched to maximum capacity per cycle mapped to `Cycle_Index`.
- **Output**: Merged capacities into `cycle_summary` for cycle-wise comparison.

### 7. C-rate Calculation 
- **Definition**: `C-rate = Current(A) / CELL_CAPACITY` (2.3 Ah).
- **Action**: Computed C-rates for full dataset, charging, and discharging phases; averaged per cycle in `cycle_summary`.
- **Purpose**: Normalized charge/discharge rates for performance analysis.

### 8. Battery Performance Visualizations 
- **Profiles**: Capacity vs. Voltage for first five cycles (charge/discharge).
- **Trends**: Charge/Discharge Capacity vs. Cycle Index to show degradation.
- **C-rate Impact**: Scatter plots of C-rate vs. Capacity with correlations (-0.833 charge, 0.667 discharge).
- **Design**: 3x2 subplot grid with clear labels and legends.

### 9. Polynomial Regression for Prediction 
- **Model**: 2nd-degree polynomial regression to capture non-linear capacity degradation.
- **Implementation**: Used `PolynomialFeatures` and `LinearRegression` to predict 20 future cycles.
- **Evaluation**: R² and MSE to ensure model accuracy.
- **Visualization**: Plotted actual vs. predicted capacities with R² scores.

### 10. State of Health (SOH) Analysis 
- **Challenge**: Integral method yielded unrealistic SOH (~2000%). Switched to maximum capacity per cycle.
- **Calculation**: `SOH = (Current Capacity / Initial Capacity) × 100%`.
- **Visualizations**:
- SOH vs. Cycle Index with 80% end-of-life threshold.
- SOH distribution and degradation rate plots.
- **Purpose**: Assessed battery longevity using industry-standard metrics.

### 11. Advanced Visualization Dashboard 
- **Capacity Fade**: Plotted percentage change in capacity per cycle.
- **Energy Efficiency**: `(Discharge Energy / Charge Energy) × 100%`.
- **SOH Comparison**: Charge vs. Discharge SOH with shaded difference area.

### 12. Correlation and Statistical Analysis 
- **Correlation Matrix**: Heatmap of Cycle_Index, Capacity, SOH, and C-rate relationships (e.g., negative Cycle-SOH correlation).
- **Statistical Tests**: Pearson tests for significance (r-values, p-values).
- **Purpose**: Quantified and validated variable relationships.

                  
> **Challenge Found**: Unable to use integral method for charge/discharge capacity as the result divulged from normal behaviour and had to use maximum capacity of each cycle index for accurate SOH calculations.

---
## Technologies Used
- **Python**: Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn
- **Environment**: Google Colab with Google Drive integration
- **Data Format**: Excel datasets on available battery time series data
---

## References
- [Scikit-learn Documentation](https://scikit-learn.org/stable/)
- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [Matplotlib Documentation](https://matplotlib.org/stable/)
- [Seaborn Documentation](https://seaborn.pydata.org/)
- [Battery University](https://batteryuniversity.com/)
- [BatteryArchive](https://www.batteryarchive.org/)
- [MathWorks Battery Visualization](https://www.mathworks.com/help/simscape-battery/ug/visualize-battery-simulation-output-data.html)