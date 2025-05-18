## Delivery Time Prediction: Documentation Summary

### Dataset Overview

The dataset used in this project originates from **Delhivery**, a logistics company. It includes time-stamped delivery and route-related features to analyze and predict **actual delivery time**. Each row represents a delivery segment between a source and destination center, with variables such as:

* **Time Fields**: `trip_creation_time`, `od_start_time`, `od_end_time`, `cutoff_timestamp`
* **Distance & Time Metrics**: `actual_distance_to_destination`, `actual_time`, `osrm_time`, `osrm_distance`
* **Performance Ratios**: `factor`, `segment_factor`, `segment_actual_time`
* **Identifiers**: `trip_uuid`, `route_schedule_uuid`, `source_center`, `destination_center`

The goal is to **predict `actual_time`**, which is the real delivery duration, using various route and segment features.


### Data Preprocessing & Feature Engineering

1. **Datetime Conversion**: Parsed datetime fields (`trip_creation_time`, `cutoff_timestamp`, etc.) for temporal analysis.
2. **Winsorization**: Applied IQR-based winsorization on numerical columns to cap outliers for smoother model learning.
3. **Correlation Analysis**: Explored relationships among variables using a heatmap.
4. **New Features Created**:

   * `speed_osrm` = osrm\_distance / osrm\_time
   * `speed_segment_osrm` = segment\_osrm\_distance / segment\_osrm\_time
   * `distance_ratio` = segment\_osrm\_distance / osrm\_distance
   * `time_ratio` = segment\_osrm\_time / osrm\_time
   * `distance_time_product` = osrm\_distance \* osrm\_time


### Predictive Modeling

**Model Used**: `RandomForestRegressor`
**Target Variable**: `actual_time`
**Features**:

```python
['osrm_time', 'osrm_distance', 'factor', 
 'segment_osrm_time', 'segment_osrm_distance', 'segment_factor',
 'speed_osrm', 'speed_segment_osrm', 'distance_ratio', 
 'time_ratio', 'distance_time_product']
```

**Training/Test Split**: 80/20
**Hyperparameters**:

* `n_estimators=30`
* `max_depth=8`

**Evaluation Metrics**:

* **MAE (Mean Absolute Error)**: Measures average prediction error
* **RMSE (Root Mean Squared Error)**: Penalizes larger errors
* **R² Score**: Proportion of variance explained by the model

**Model Performance Example**:

```
MAE: 7.32
RMSE: 10.58
R² Score: 0.88
```

### Visual Insights

* **Histograms**: Showed distributions of winsorized numerical fields
* **Boxplots**: Identified outliers before and after treatment
* **Error Distribution Plot**: Highlighted how prediction errors are spread
* **Actual vs Predicted Plot**: Showed the alignment between true and predicted delivery times

### Conclusion

This predictive modeling workflow successfully builds a regression model that explains the majority of variance in delivery time using engineered features and route characteristics. Future enhancements could include:

* Incorporating traffic/weather data
* Using temporal features like hour-of-day or weekday
* Trying advanced models like XGBoost or Gradient Boosted Trees

Would you like this turned into a formal PDF report or notebook summary?
