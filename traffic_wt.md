# Wavetronix (Radar)

Wavetronix (radar) is a nonintrusive vehicle detector that provides information on traffic volume and speed. The dataset consists of 15-minute interval counts and speeds-per-lane. It provides details of mid-block traffic volume and speed per travel direction in aggregate form. Valuable insights for planning and evaluation can be derived by allowing comparisons of system use and performance across time periods (e.g., before, during, and after construction) and/or time of day.

### Table Description

The Wavetronix radar dataset is composed of two tables: one describes the location of the sensors and a second holds the data itself.

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
wt_intersections|id|text|Wavetronix sensor ID
wt_intersections|name|text|Name given to sensor
wt_intersections|int_geom_id|integer|Intersection ID where the sensors is located

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
wt_data|id|text|Wavetronix sensor id. References `wt_intersections`
wt_data|timestamp|timestamp|Timestamp of counts in 15-minute intervals
wt_data|lane|text|Lane at which counts were detected
wt_data|direction|text|Direction at which counts were detected
wt_data|volume|integer|Traffic volume (mid-block) within 15-minute interval
wt_data|speed|numeric|Traffic average speed (mid-block) within 15-minute interval
<br></br>

### Data Considerations

Because the data is highly localized (point speeds and volumes), explaining detected trends may require the analysis of several sensors, or obtaining additional data from separate sources. Additionally, missing data is not uncommon for for some locations, dates or time of day. Procedures to detect missing data (skipped time intervals) are in place within CTR.
