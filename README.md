# Forecasting Energy Usage Consumption

## School of Information Studies, IST 687 – Data Science  
**Group 3:** Harish Reddy Yeddula, Kunal Jain, Nikitha Chandana, Raghuveera Narasimha, Rishi Siddanth Yaga

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Data Merging](#data-merging)
3. [Data Summarization by Day](#data-summarization-by-day)
4. [Data Cleaning](#data-cleaning)
5. [Data Exploration](#data-exploration)
6. [Data Modelling](#data-modelling)
7. [5 Degree Warmer Prediction](#5-degree-warmer-prediction)
8. [Key Data Insights](#key-data-insights)
9. [Shiny App Results](#shiny-app-results)
10. [Recommendations](#recommendations)
11. [Conclusion](#conclusion)

---

## Project Overview  
Developed a predictive model for eSC Energy to forecast and manage energy consumption, especially during July peak demand, considering global warming impacts. Aimed to prevent blackouts and unnecessary infrastructure expansion. Delivered a report, model, and Shiny application with data-driven energy conservation recommendations.

---

## Data Merging  
**Purpose:** Merge house, energy, and weather data per house ID.

### Steps:
- Load libraries (`arrow`, `dplyr`, `ggplot2`).
- Read static house info (Parquet from S3).
- Extract house IDs.
- Loop through IDs:
  - Read energy data (dynamic URL).
  - Filter July 2018 energy data.
  - Calculate total energy consumed.
  - Merge energy, house, and weather data.
  - Append to list.
- Combine data frames.
- Save merged data (Parquet).

**Challenges:** Dynamic data sources, progress tracking (handled with `tryCatch`, counters).

---

## Data Summarization by Day  
**Purpose:** Summarize merged data daily into `data_summarized`.

### Steps:
- Read merged data (Parquet).
- Convert time to Date (day column).
- Group by building ID, time, county.
- Summarize energy, production, weather stats.
- Merge with static house info.
- Save summarized data (Parquet).

**Challenges:** Data volume (addressed with `dplyr` efficiency).

---

## Data Cleaning  
**Purpose:** Preprocess data: missing values, data types, features.

### Steps:
- Read summarized data (Parquet).
- Create data copy.
- Remove zero variance columns.
- Convert income ranges to means (`range_to_mean`).
- Convert numeric variables to numeric.
- Create `energy_usage_group`, `building_size` columns.
- Impute missing numeric (mean), categorical (mode).
- Handle non-standard missing values (to NA).
- Calculate missing data percentage per column.
- Remove columns >70% missing.
- Replace remaining NAs with mode.
- Save cleaned data (Parquet).

**Challenges:** Data heterogeneity (handled with custom functions, imputation).

---

## Data Exploration  
**Purpose:** Visualize data patterns and relationships.

### Steps:
- Read cleaned data (Parquet).
- Fuel type distribution by city (bar plot).
- Energy consumption (1 vs 2 story buildings - boxplot).
- Correlation heatmap (weather & energy variables).
- Avg temp & electricity over time (line plot, dual axes).
- Net energy consumption by city (bar plot).

**Challenges:** Visualization complexity (targeted visualizations used).

---

## Data Modelling  
**Purpose:** Build linear regression model for energy consumption prediction.

### Steps:
- Read cleaned data (Parquet).
- Convert negative energy values to zero.
- Remove ID & energy variable columns.
- Remove zero variance columns.
- Split data (train/test - `caret`).
- Build linear regression model (`lm`).
- Predict on test set & evaluate (MAE, MSE, RMSE, R-squared).

**Challenges:** Model selection (linear regression chosen for linear relationships).

---

## 5 Degree Warmer Prediction  
**Purpose:** Simulate +5°C temperature impact on energy consumption.

### Steps:
- Read cleaned data (Parquet).
- Create dataset with +5°C temperature.
- Predict energy consumption using linear model.

**Challenges:** Temperature simulation (data modification for scenario analysis).

---

## Key Data Insights  
- **Wall Type & Energy:** Wood frame walls show higher energy consumption compared to brick, concrete, steel frame. Steel frames show the lowest.
- **Wall Exterior:** Moderate energy variation by exterior type. Medium-colored Fiber Cement/Stucco showed higher consumption.
- **Square Footage:** Energy consumption increases with building size, significant jump over 6000 sq ft.
- **Bedrooms:** Median energy consumption increases with bedroom count; larger homes show more consumption variability.
- **Temperature Correlation:** Moderate positive correlation (0.51) between bedroom temperature and energy consumption, stronger in summer & larger homes.
- **Weather Impact:** Dry bulb temperature strongly positively correlated with electricity use. Humidity & radiation also positive.
- **Fuel Type Distribution:** Significant city-to-city fuel type variation.
- **City Consumption:** Large variations in total energy consumption across cities.

---

## Shiny Application Results  
Interactive web app displays temperature and energy consumption over time, allowing users to explore data by county/building. Temperature fluctuates daily. Energy consumption shows patterns, potentially correlated with temperature and building usage.

---

## Recommendations  
1. **Focus on High Energy Consumers:** Prioritize "Very High" usage buildings.
2. **Target High Consumption Cities:** Implement initiatives in high net consumption cities.
3. **Building Size Strategies:** Tailor strategies for small, medium, large buildings.
4. **Weather-Specific Measures:** Implement temperature-based energy saving actions.
5. **Renewable Energy Incentives:** Expand renewables in effective producer buildings.
6. **Optimize HVAC:** Based on temperature impact analysis.
7. **Building-Specific Audits:** For high consumption buildings (insulation, HVAC, appliances).
8. **Behavioral Education:** Promote energy-conscious habits.
9. **Predictive Maintenance:** For efficient energy equipment.
10. **Continuous Monitoring:** For anomaly detection & efficiency.
11. **Community Collaboration:** Engage local entities in high usage areas.
12. **Incentive Programs:** Reward energy efficiency.

---

## Conclusion  
Project delivered a robust framework for energy data analysis, demonstrating proficiency in data handling, modeling, and visualization. Scalable design adaptable to evolving energy data trends, providing eSC with actionable insights for informed decision-making, energy conservation, and sustainable resource management.
