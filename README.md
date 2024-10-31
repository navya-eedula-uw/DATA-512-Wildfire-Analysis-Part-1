# DATA-512: Wildfire Analysis (Part-1)

More and more frequently summers in the western US have been characterized by wildfires with smoke billowing across multiple western states. There are many proposed causes for this: climate change, US Forestry policy, growing awareness, just to name a few. Regardless of the cause, the impact of wildland fires is widespread as wildfire smoke reduces the air quality of many cities. There is a growing body of work pointing to the negative impacts of smoke on health, tourism, property, and other aspects of society.
The course project will require that you analyze wildfire impacts on a specific city in the US. The end goal is to be able to inform policy makers, city managers, city councils, or other civic institutions, to make an informed plan for how they could or whether they should make plans to mitigate future impacts from wildfires.

### License

Many code example notebooks was developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.1 - August 16, 2024

The following functions and pieces of code were used from Dr. David W. McDonald's Python Notebook File `wildfire_geo_proximity_example.ipynb`:  
1. `convert_ring_to_epsg4326`
2. `shortest_distance_from_place_to_fire_perimeter`
3.  API Request Template
4. `request_signup`
5. `request_list_info`
6. `request_daily_summary`

The rest of the code lies under the standard MIT license.

### Details about Centennial, Colarado

| City       | ST | 2023 Estimate | 2020 Census | 2020 Density (mi²) | Location         |
|------------|----|---------------|-------------|---------------------|-------------------|
| Centennial | CO | 106,883       | 108,418     | 3,650               | 39.59°N 104.87°W  |

# API Documentation

## 1. USGS Wildfire Dataset API

The Combined Wildland Fire Dataset API provides access to historical wildfire data across the United States from the 1800s to present.Dataset Access: [https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81)

## 2. EPA Air Quality System (AQS) API
The EPA AQS API provides historical air quality data across the United States, with standardized measurements beginning in the 1980s.

```
https://aqs.epa.gov/data/api/
```

### Authentication
- Requires API key
- [Request API key here](https://aqs.epa.gov/data/api/signup)
- Key must be included in all requests

### Rate Limits
- Please refer to [EPA's AirData FAQ](https://www.epa.gov/outdoor-air-quality-data/frequent-questions-about-airdata) for current rate limits

# Project Workflow

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

# Wildfire Data
### Dataset
The [Combined wildland fire datasets for the United States and certain territories](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81), [1800s-Present (combined wildland fire polygons)](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) dataset has details about all wildfires that have happened over the years all over the US. This dataset was collected and aggregated by the US Geological Survey. The dataset is relatively well documented. The dataset provides fire polygons in ArcGIS and GeoJSON formats. 
This dataset is almost 2GB in size. Due to file upload limiations on Github, I've not included the dataset in the repository. However, one should be able to find it easily using the above links.

### Data Acquisation: Wildfires in Centennial, CO
The code for this can be found in the [data_acquisation.ipynb](data_acquisation.ipynb) notebook. Running this notebook end-to-end should produce the necessary wildfire data for the areas surrounding Centennial, CO. 

The required output is stored as a CSV file in [intermediate_files/wildfires_with_distances.csv](intermediate_files/wildfires_with_distances.csv). 
To analyze only relevant wildfires that are closer to the city, I also created [intermediate_files/wildfire_df_within_650_miles.csv](intermediate_files/wildfire_df_within_650_miles.csv) which filters the wildfires within a 650 mile radius of Centennial, CO. 

# AQI Data
### Dataset
The dataset is sourced from the [U.S. Environmental Protection Agency's (EPA) Air Quality System (AQS) API](https://aqs.epa.gov/aqsweb/documents/data_api.html), which provides historical air quality measurements across the United States. While not offering real-time data, this comprehensive database began standardized monitoring with quality assurance procedures in the 1980s, following the EPA's establishment in the early 1970s. The data collection typically initiated between 1983-1988 for most counties, though coverage varies geographically as some regions still lack monitoring stations. The API offers flexible data retrieval options through station IDs, county designations, or geographic bounding boxes, making it particularly valuable for spatial analysis. The measurements contribute to calculating the Air Quality Index (AQI), a key public health indicator frequently referenced in weather reports and environmental assessments, particularly during smog or smoke events. The technical specifications for AQI calculations follow the [EPA's Technical Assistance Document](https://www.airnow.gov/sites/default/files/2020-05/aqi-technical-assistance-document-sept2018.pdf), ensuring standardized interpretation of air quality conditions. For additional context about the data collection and system specifications, users can refer to the [EPA's AirData FAQ](https://www.epa.gov/outdoor-air-quality-data/frequent-questions-about-airdata), which provides comprehensive background information about the monitoring network and data collection protocols.

### Calculating Annaul Estimate of AQI
The code for this can be found in the [epa_air_quality_history_example.ipynb](epa_air_quality_history_example.ipynb) notebook. Running this notebook end-to-end should produce the annual AQI estimate values for Centennial, CO.

The required output is stored as a CSV file in [intermediate_files/wildfires_with_distances.csv](intermediate_files/wildfires_with_distances.csv).

# Fomulating Smoke Estimate

I created an annual estimate of wildfire smoke for my assigned city. This estimate served as a numerical value that I could eventually use to develop a predictive model. While technically, the impact of smoke should have encompassed health, tourism, economic, and other social issues resulting from it, I referred to my estimate as the wildfire smoke impact. I planned to explore other potential social and economic effects in Part 2 of the course project. For that moment, I needed a figure to represent the estimated smoke my city experienced during each annual fire season.

Why was this an estimate of fire smoke? These figures were estimates due to several challenges that weren't easy to address, combined with necessary simplifications to make the course project manageable within a few weeks. For example, an accurate smoke impact assessment would have taken into account factors like wind direction over several days, the intensity of the fire, and its duration. However, the fire polygon data only provided a reliable year for each fire, without specific start and end dates.

The code for this can be found in the [smoke_estimate.ipynb](smoke_estimate.ipynb) notebook. Running this notebook end-to-end should produce smoke estimate values originted from wildfires from the years 1964-2020 wihin 650 miles of Centennial, CO.

![image](https://github.com/user-attachments/assets/97ded08d-bf3e-45eb-a5b6-db3442b2add9)

The above line chart shows the trends of two different variables—Average AQI (Air Quality Index) and Smoke Estimates—over time, from 1964 to 2024. Both variables are normalized on a scale from 0 to 1 to allow comparison.


### Methodology
I formulated the smoke estimate by incorporating several factors, starting with the **size factor**, which uses a logarithmic transformation of the fire's area in acres to normalize values and assess its correlation with smoke production. The **duration factor** estimates the burn duration based on the fire's perimeter and area, capping the maximum duration at 30 days to reflect realistic limits. Additionally, the **circleness scale** measures the perimeter's circularity, suggesting that more circular fires tend to produce more smoke due to uniform fuel and wind conditions. A categorical variable based on fire classification assigns weights to different fire types, giving higher importance to uncontrolled wildfires. The **distance decay factor** quantifies the impact of proximity to the fire, using a quadratic decay function to reflect reduced smoke effects with increased distance. The final smoke impact calculation combines these factors multiplicatively, offering a comprehensive estimate. Although the model includes several parameters derived from domain knowledge, limitations exist, such as the simplification of atmospheric dynamics and the lack of direct weather data. Future enhancements could involve incorporating additional variables like fuel type, terrain complexity, and real-time weather conditions to improve accuracy and reliability.

The required output is stored as a CSV file in [intermediate_files/smoke_estimate_data.csv](intermediate_files/smoke_estimate_data.csv)

# Time Series Model

The code for this can be found in the [modelling.ipynb](modelling.ipynb) notebook. Running this notebook end-to-end should produce the timeseries model used to forecast smoke estimates till the year 2050. 

In this analysis I used an ARIMAX (AutoRegressive Integrated Moving Average with Exogenous Variables) model to forecast wildfire smoke impacts, integrating external variables like wildfire data into the temporal forecasting framework. This model combines the strengths of time series analysis and regression, capturing both the temporal patterns of smoke estimates and the influences of wildfires on air quality.

The rationale behind using ARIMAX was to leverage known external factors affecting smoke estimates over time, particularly during wildfire months when significant variations in smoke levels occur. While the model shares wildfire data for both estimating and forecasting, this strategy aims to enhance predictive accuracy by capturing trends not reflected in initial estimates.
![image](https://github.com/user-attachments/assets/649703ed-6f71-4e76-9c3c-e3ff8158e3eb)

The forecast results indicated an upward trend in smoke impact forecasts, consistent with historical data showing increasing fire sizes and frequencies.





