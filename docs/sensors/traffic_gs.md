# GRIDSMART

GRIDSMART cameras are fish-eye cameras mounted high above an intersection. The cameras use automated video data processing to identify vehicles crossing through defined regions, called zones, in an intersection. The resulting data may be processed to generate volumes per approach, turning movement counts, speeds, and delays, among others parameters. We document the aggregate data structure.

### Table Description

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
gs_intersections|id|integer|GRIDSMART camera ID
gs_intersections|street1|text|Main street of intersection where camera is located
gs_intersections|street2|text|Cross street of intersection where camera is located
gs_intersections|lat|numeric|Latitude of sensor location (SRID EPSG:4326)
gs_intersections|lon|numeric|Longitude of sensor location (SRID EPSG:4326)

<br></br>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
gs_movement|id|integer|GRIDSMART camera ID
gs_movement|intersection|integer|Intersection ID
gs_movement|guid|text|ID of pre-defined detection zone
gs_movement|zone_turn|text|Turns associated with detection zone
gs_movement|zone_approach|text|Approach associated with detection zone

<br></br>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
gs_agg_data|intersection|integer|Intersection ID. References `gs_movement`
gs_agg_data|timestamp|timestamptz|timestamp of counts in 15-minute intervals. Time zone is stored in UTC and should be converted to local time (US/Central)
gs_agg_data|zone_approach|text|Approach of counts. On of Northbound (NB), Southbound (SB), Eastbound (EB), Westbound (WB)
gs_agg_data|turn|text|Turn movement of counts. One of left (L), right (R), straight (S), U-turn (U)
gs_agg_data|heavy_vehicle|integer|0-1 variable if counts are for vehicles > 17 ft
gs_agg_data|volume|integer|Traffic counts at intersection

<br></br>

### Data Considerations

* It is difficult to know how the accuracy of the GRIDSMART information. Some of the images are significantly distorted or low-resolution and, at times, intersections may suffer from a huge gap in data at particular hours
* GRIDSMART is geared toward measuring vehicular traffic and it is not suitable for measuring pedestrian or bicycle traffic
* The distribution of raw speed values have long tails. Median speed values are more realistic than mean values for data aggregation.
