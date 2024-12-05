## Wildfire Analysis - Part 1

### Overview

This repository contains the first part of a project aimed at analyzing wildfires using data-driven approaches. The project is part of the DATA 512 course and focuses on exploring wildfire patterns, trends, and their implications through data analysis and visualization techniques.

More and more frequently summers in the western US have been characterized by wildfires with smoke billowing across multiple western states. There are many proposed causes for this: climate change, US Forestry policy, growing awareness, just to name a few. Regardless of the cause, the impact of wildland fires is widespread as wildfire smoke reduces the air quality of many cities. There is a growing body of work pointing to the negative impacts of smoke on health, tourism, property, and other aspects of society.

The primary goal of this project is to analyze wildfire data to uncover trends, patterns, and potential causes. This analysis can help in understanding the impact of wildfires on the economy in Centennial, Colorado and the broader Denver-Aurora-Lakewood region, while providing insights for policymakers to mitigate wildfire risks.


The course project required me to analyze wildfire impacts on a specific city in the US. The end goal is to be able to inform policy makers, city managers, city councils, or other civic institutions, to make an informed plan for how they could or whether they should make plans to mitigate future impacts from wildfires.

---

### Installation and Setup

To run this project locally, follow these steps:

1. Clone the repository:
   ```
   git clone https://github.com/navya-eedula-uw/DATA-512-Wildfire-Analysis-Part-1.git
   ```

2. Navigate to the project directory:
   ```
   cd DATA-512-Wildfire-Analysis-Part-1
   ```

---

### Usage

To execute the analysis:

1. Open each of the Jupyter Notebook file in your preferred environment:
   ```
   jupyter notebook wildfire_analysis_part1.ipynb
   ```

2. Run each cell sequentially to:
   - Load and preprocess data
   - Perform exploratory data analysis (EDA)
   - Generate visualizations

3. Modify parameters or add custom code as needed to explore additional insights.

---

### File Structure

```
DATA-512-WILDFIRE-ANALYSIS-PART-1
├── documents
│   └── DATA 512_ Part 1 - Common Analysis_ Reflection Statement.pdf
│   └── Pecha Kucha Presentation.pdf
│   └── Part 2 - An Extension Plan - Navya Eedula.pdf
├── economy_data
│   ├── annual_consumer_price_index.csv
│   ├── annual_per_capita_personal_income.csv
│   ├── annual_population_data.csv
│   ├── monthly_active_home_listings.csv
│   ├── monthly_market_hotness_ranking.csv
│   ├── monthly_median_price_per_square_foot.csv
│   ├── monthly_new_home_construction_permits.csv
│   ├── monthly_unemployment_rate.csv
│   └── monthly_wage_levels_for_private_sector.csv
├── economy_indicators_analysis
│   ├── consumer_goods_market_analysis.ipynb
│   ├── housing_market_analysis.ipynb
│   └── wages_and_employment_analysis.ipynb
├── intermediate_data_files
│   ├── aqi_yearly.csv
│   ├── smoke_estimate_data.csv
│   ├── smoke_estimates_and_predictions.csv
│   ├── wildfire_df_within_650_miles.csv
│   └── wildfires_with_distances.csv
├── modeling_and_forecasting_smoke_estimate
│   ├── .ipynb_checkpoints
│   ├── data_acquisition.ipynb
│   ├── data_visualizations.ipynb
│   ├── epa_air_quality_history_example.ipynb
│   ├── modelling.ipynb
│   └── smoke_estimate.ipynb
├── LICENSE
└── README.md

```

---

### Dependencies

## Python Libraries and Dependencies

1. **`pandas`** *(Needs Installation)*
   A library for data manipulation and analysis, ideal for handling tabular data with DataFrames.
   - Installation:
     ```bash
     pip install pandas
     ```

2. **`os`** *(Inbuilt)*
   Provides utilities to interact with the operating system, such as file handling and accessing environment variables.

3. **`plotly.graph_objects`** and **`plotly.subplots`** *(Needs Installation)*
   Used for creating interactive visualizations and plotting diagnostic plots.
   - Installation:
     ```bash
     pip install plotly
     ```

4. **`seaborn`** *(Needs Installation)*
   A statistical data visualization library built on `matplotlib` for creating attractive and informative plots.
   - Installation:
     ```bash
     pip install seaborn
     ```

5. **`matplotlib.pyplot`** *(Inbuilt in `matplotlib`, Needs Installation)*
   A library for creating static, animated, and interactive visualizations.
   - Installation:
     ```bash
     pip install matplotlib
     ```

6. **`scipy.stats`** *(Inbuilt in `scipy`, Needs Installation)*
   Provides statistical functions and distributions for data analysis.
   - Installation:
     ```bash
     pip install scipy
     ```

7. **`sklearn.preprocessing.MinMaxScaler`** *(Inbuilt in `scikit-learn`, Needs Installation)*
   Normalizes data within a specified range (e.g., 0 to 1) for machine learning tasks.
   - Installation:
     ```bash
     pip install scikit-learn
     ```

8. **`statsmodels.tsa.statespace.SARIMAX`** *(Inbuilt in `statsmodels`, Needs Installation)*
   Implements SARIMAX (Seasonal AutoRegressive Integrated Moving Average with eXogenous variables) for time series forecasting.
   - Installation:
     ```bash
     pip install statsmodels
     ```

9. **`sklearn.linear_model.LinearRegression`** *(Inbuilt in `scikit-learn`, Needs Installation)*
   Provides tools for fitting linear regression models for predictive tasks.
   - Installation:
     ```bash
     pip install scikit-learn
     ```

10. **`numpy`** *(Needs Installation)*
    A fundamental library for numerical computing, offering multi-dimensional arrays and mathematical functions for array operations.
    - Installation:
      ```bash
      pip install numpy
      ```
---
# Wildfire Data
### Dataset
The [Combined wildland fire datasets for the United States and certain territories](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81), [1800s-Present (combined wildland fire polygons)](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) dataset has details about all wildfires that have happened over the years all over the US. This dataset was collected and aggregated by the US Geological Survey. The dataset is relatively well documented. The dataset provides fire polygons in ArcGIS and GeoJSON formats.
This dataset is almost 2GB in size. Due to file upload limiations on Github, I've not included the dataset in the repository. However, one should be able to find it easily using the above links.

# AQI Data
### Dataset
The dataset is sourced from the [U.S. Environmental Protection Agency's (EPA) Air Quality System (AQS) API](https://aqs.epa.gov/aqsweb/documents/data_api.html), which provides historical air quality measurements across the United States. While not offering real-time data, this comprehensive database began standardized monitoring with quality assurance procedures in the 1980s, following the EPA's establishment in the early 1970s. The data collection typically initiated between 1983-1988 for most counties, though coverage varies geographically as some regions still lack monitoring stations. The API offers flexible data retrieval options through station IDs, county designations, or geographic bounding boxes, making it particularly valuable for spatial analysis. The measurements contribute to calculating the Air Quality Index (AQI), a key public health indicator frequently referenced in weather reports and environmental assessments, particularly during smog or smoke events. The technical specifications for AQI calculations follow the [EPA's Technical Assistance Document](https://www.airnow.gov/sites/default/files/2020-05/aqi-technical-assistance-document-sept2018.pdf), ensuring standardized interpretation of air quality conditions. For additional context about the data collection and system specifications, users can refer to the [EPA's AirData FAQ](https://www.epa.gov/outdoor-air-quality-data/frequent-questions-about-airdata), which provides comprehensive background information about the monitoring network and data collection protocols.

### Economy Data for the Denver-Aurora-Lakewood Region from the FRED Reserve Bank of St.Louis
The multiple datasets from the [FRED Reserve Bank of St.Louis](https://fred.stlouisfed.org/) cover a range of economic indicators for the Denver-Aurora-Lakewood, CO metropolitan area (CBSA/MSA) over various time periods. This comprehensive dataset, covering housing, labor, development, wages, prices, and population, will enable a robust analysis of how wildfire smoke events have influenced the economic health and resilience of the Denver-Aurora-Lakewood metropolitan area. By examining these indicators before, during, and after major wildfire occurrences, I can hopefully develop a more nuanced understanding of the far-reaching socioeconomic impacts beyond the immediate physical effects.

## Data License
The wildfire data utilized in this analysis is sourced from public datasets such as the [USGS Wildland Fire Dataset](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) and the [EPA's Air Quality System (AQS) API](https://aqs.epa.gov/aqsweb/documents/data_api.html). The data is shared under the respective licenses provided by these organizations and is intended for public use and research purposes.

For economic data, we use information from the [FRED Reserve Bank of St. Louis](https://fred.stlouisfed.org/). Users must adhere to their data usage terms and policies.

By using this repository, you agree to comply with the licenses and terms set by these data providers.

## Code License
Many code example notebooks was developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.1 - August 16, 2024
The following functions and pieces of code were used from Dr. David W. McDonald's Python Notebook File `wildfire_geo_proximity_example.ipynb`:
1. `convert_ring_to_epsg4326`
2. `shortest_distance_from_place_to_fire_perimeter`
3.  API Request Template
4. `request_signup`
5. `request_list_info`
6. `request_daily_summary`
The rest of the code lies under the standard MIT license.

## API Documentation
### Wildfire Data API
The wildfire data is accessed through the [USGS Wildfire Dataset](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) and includes data in GeoJSON and ArcGIS formats.

### Air Quality Data API
Air quality data is retrieved using the [EPA AQS API](https://aqs.epa.gov/aqsweb/documents/data_api.html). Detailed API documentation, including endpoints and query parameters, is available [here](https://www.epa.gov/outdoor-air-quality-data).

## Research Implications
This project sheds light on:
1. The socioeconomic impact of wildfires, particularly in the Denver-Aurora-Lakewood metropolitan area.
2. The relationship between wildfire smoke and air quality, housing markets, and labor dynamics.
3. Insights for policymakers to create more effective wildfire mitigation strategies

# Project Workflow
## modeling_and_forecasting_smoke_estimate
1. **data_acquisition.ipynb**
This notebook retrieves the wildland fire dataset from the USGS, computes the distance of each fire from Centennial, filters the dataset for the years 1964 to 2024, and produces two CSV files containing selected columns. One file includes all fires that occurred during this time frame, while the other focuses on fires within a 650-mile radius of Madison.

2. **epi_air_quality_history_example.ipynb**
This notebook is designed to obtain AQI data from an API, clean the dataset, calculate any missing AQI values, and ultimately generate a CSV file containing yearly AQI estimates from 1964 to 2024.

3. **smoke_estimate.ipynb**
This notebook utilizes the files created by the previous notebooks to calculate smoke estimates and compare these estimates with AQI data for validation purposes.

4. **modelling.ipynb**
This notebook is employed to develop a time series forecasting model based on smoke estimates, aiming to forecast smoke levels for the next 25 years.

5. **data_visualizations.ipynb**
This notebook analyzes the data generated thus far using visual representations. It creates three specific visualizations as outlined in the project requirement document.

## economy_indicators_analysis
1. **consumer_goods_market_analysis.ipynb**
This notebook presents an analysis related to consumer behavior or market conditions; Annual Consumer Price Index, an indicator of inflation and purchasing power; and Monthly Market Hotness Ranking, a gauge of market activity and consumer interest.
2. **housing_market_analysis.ipynb**
This notebook presents a comprehensive analysis of housing market trends and their potential relationship with smoke estimates. The dataset includes key housing market indicators such as monthly active home listings, median price per square foot, and new home construction permits.
3. **wages_and_employment_analysis.ipynb**
The notebook examines the forecasted smoke estimate alongside economic indicators such as annual per capita personal income, monthly unemployment rate, and monthly wage levels for the private sector.




### Contributing

Contributions to this project are welcome! To contribute:

1. Fork the repository.
2. Create a new branch for your feature or bug fix:
   ```bash
   git checkout -b feature-name
   ```
3. Commit your changes:
   ```bash
   git commit -m "Description of changes"
   ```
4. Push to your branch:
   ```bash
   git push origin feature-name
   ```
5. Submit a pull request for review.

---



