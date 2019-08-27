# Bluetooth
Bluetooth sensors are spread within City of Austin limits. The raw data is processed to attain travel time information for roadways throughout the city. Bluetooth sensors at specific locations detect Bluetooth-enabled mobile devices. When a mobile device is detected, the system logs the ID of the Bluetooth sensor, the timestamp of detection as well as the anonymized Media Access Control (MAC) address of the detected device. From this raw information, travel times are computed by identifying the same mobile device at two locations along a roadway, and taking the difference between timestamps.

There are several types of Bluetooth data files reported, ranging from the very raw data to more aggregate data along specified roadway segments. Here, we focus documenting the a) sensor location information, b) the raw data structure, c) the daily aggregate travel times computed using CTR methodologies and, lastly, d) supporting tables for computation. We describe the CTR methodology in the [Bluetooth Processed](#bt_processed) section of this document.

### Bluetooth Sensor Locations Table Description

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
bt_intersections|id|integer|Sensor ID given to Bluetooth sensor
bt_intersections|reader_id|text|Name given to Bluetooth sensor
bt_intersections|int_geom_id|integer|Intersection ID where the Bluetooth sensors is located
bt_intersections|lat|numeric|Latitude of sensor location (SRID EPSG:4326)
bt_intersections|lon|numeric|Longitude of sensor location (SRID EPSG:4326)

<br></br>

### Raw Bluetooth Table Description

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
bt_data_unmatched|int_id|integer|Intersection ID where the Bluetooth sensors is located. References `bt_intersections`
bt_data_unmatched|det_time|timestamp|Timestamp of detection
bt_data_unmatched|device_addr|text|Anonymized MAC address of detected mobile device

<br></br>

### <a id="bt_processed"></a>Processed Bluetooth Table Description

CTR processes raw Bluetooth data to compute daily travel times at 15-minute intervals between consecutive sensors along the nine [Bond Corridors](https://data.austintexas.gov/stories/s/Corridor-Mobility-Program/gukj-e8fh/). '2-point' and '4-point' methods are implemented along with the Median Absolute Deviation (MAD) procedure for travel time data cleaning.

The '2-point' method calculates the travel time between two consecutive sensors. It includes vehicles conducting all movements. For example, vehicles waiting to turn left at an intersection. In contrast, the '4-point' method utilizes those vehicles going through the entire segment to estimate travel time. This is achieved by selecting vehicles that are detected at four locations: the two consecutive Bluetooth sensors for which the roadway segment is being considered as well the sensors directly before and after the segment. The MAD procedure is a statistical cleaning method to detect outliers. Travel time estimation outliers may stem from behavior such as vehicles stopping mid-way along the segment for some period of time.

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
bt_daily_agg|tod|text|Time of day in 'HH:MM' format at 15-minute intervals
bt_daily_agg|dir|text|One of 'Direction 1' or 'Direction -1'. Serves to distinguish between directions based on sensor sequence.
bt_daily_agg|id_o|integer|Sensor ID of 'origin' location. References `bt_intersections`.
bt_daily_agg|id_d|integer|Sensor ID of 'destination' location. References `bt_intersections`.
bt_daily_agg|ttime_mean|numeric|Mean travel time between origin and destination sensors. Computed using methodology described above
bt_daily_agg|tt_sd|numeric|Standard deviation of travel time
bt_daily_agg|ttime_count|integer|Number of vehicles that inform the travel time estimation
bt_daily_agg|date|text|Date of observation in 'YYYY-mm-dd' format
bt_daily_agg|method|text|One of '2-point' or '4-point'. Describes the estimation procedure implemented to attain travel time value
bt_daily_agg|corr_id|integer|Corridor for which the estimation applies

<br></br>

### Supporting Tables Description

The following two tables serve to map travel time data onto roadways so that the data can be further aggregated to corridors consisting of several segments: `bt_sensor_sequence` and `bt_segments`.

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
bt_sensor_sequence|id|integer|Sensor ID. References `bt_intersections`
bt_sensor_sequence|corr_id|integer|Corridor in which the sensor is located
bt_sensor_sequence|seq|integer|Order of consecutive sensors along the corridor

<br></br>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
bt_segments|corr_id|integer|Sensor ID. References `bt_intersections`
bt_segments|direction|text|Direction of travel. One of 'Southbound', 'Northbound', 'Eastbound' or 'Westbound'.
bt_segments|segment|integer|Order of consecutive Bluetooth segments along the corridor
bt_segments|id_o|integer|Sensor ID of 'origin' location. References `bt_intersections`.
bt_segments|id_d|integer|Sensor ID of 'destination' location. References `bt_intersections`.
bt_segments|dir|text|One of 'Direction 1' or 'Direction -1'. Refers to `dir` column in `bt_daily_agg`.

<br></br>

### Data Considerations
Because not all travelers enable Bluetooth on mobile devices, the dataset encompasses only a subset of all traffic, estimated to be 2.02% to 8.13% of vehicles on the road[^1].

[^1]: Sharifi et al., “Analysis of Vehicle Detection Rate for Bluetooth Traffic Sensors: A Case Study in Maryland and Delaware,” 18th World Congress on Intelligent Transport Systems (Oct. 2011).
