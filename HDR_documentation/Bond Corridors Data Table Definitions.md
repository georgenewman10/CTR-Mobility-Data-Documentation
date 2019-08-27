# Bond Corridors Data Table Definitions

## Table of Contents

|           **Table Name**         |   **Description**   |
| :-------------------------: | :----------|
| [bt_daily_agg](#blue)             | Aggregated Bluetooth data in 15-minute intervals|
| [bt_segments](#blue)              | Bluetooth segments, defined as two consecutive sensors along corridors|
| [bt_sensor_sequence](#blue)       | Bluetooth sensors along corridors|
| [corridors](#corridors)  | This table holds the Bond Corridors information|
| [gs_agg_data](#grid)              | Aggregated GRIDSMART data in 15-minute intervals|
| [gs_intersections](#grid)         | Links camera locations to corridor intersections|
| [gs_movements](#grid)             | Defines zone approaches and turn movements for GUIDS found within device metadata|
| [gtfs_metadata](#gtfs)            | GTFS versioning scheme|
| [gtfs_routes](#gtfs)              | GTFS routes   |
| [gtfs_shapes](#gtfs)              | GTFS shapes    |
| [gtfs_shapes_geom](#gtfs)         | GTFS shapes geometry. It is processed by CTR from gtfs_shapes to facilitate analyses.|
| [gtfs_stop_times](#gtfs)          | GTFS stop times|
| [gtfs_stops](#gtfs)               | GTFS stops  |
| [gtfs_trips](#gtfs)               | GTFS trips   |
| [intersections](#intersections)            | Intersections along corridors. It includes ID and coordinates|
| [matchup_int_corr](#corridors) | Links intersections to corridors  |
| [matchup_stop_corr](#corridors)        | Links GTFS stops to corridors|
| [matchup_trip_corr](#corridors)        | Links GTFS trops to corridors (based on geometry)|
| [metdat_intersections](#intersections)     | Serves to link traffic counts to intersections|
| [metdat_traffic_counts](#counts)    | Metadata on traffic counts|
| [traffic_counts](#counts)           | Traffic count data|
| [transit_apc](#apc)         | Automated Passenger Counts (APC) data pertinent to corridors|
| [transit_avl](#avl)       | Automated Vehicle Locations (AVL) data pertinent to corridors|
| [wt_data](#wave)                  | Wavetronix (radar) data|
| [wt_intersections](#wave)         | Wavetronix (radar) sensor locations. It links intersections to sensor locations|

## <a id="corridors">Corridors</a>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|corridors|gid|integer|ID given by database upon ingestion|
|corridors|corr_id|integer|Corridor identifier used to link other data to corridors|
|corridors|corr_name|text|Name of corridor|
|corridors|start_int|text|Intersection at which the start of the corridor is defined|
|corridors|end_int|text|Intersection at which the end of the corridor is defined|
|corridors|dir0|text|Direction from start intersection to end intersection|
|corridors|dir1|text|Direction opposite as `dir0`|
|corridors|geom|geometry(LineString,4326)|Geometry of corridor|

<br> </br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|matchup_int_corr|int_id|integer|Intersection identifier|
|matchup_int_corr|corr_id|integer|Corridor identifier|

<br> </br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|matchup_stop_corr|corr_id|integer|Corridor identifier|
|matchup_stop_corr|gtfs_version|text|GTFS version identifier|
|matchup_stop_corr|stop_id|integer|GTFS stop identifier|
|matchup_stop_corr|stop_dir|text|Direction along corridor where stop lives. "N" for northbound, "S" for southbound, "E" for Eastbound, "W" for westbound|

<br> </br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|matchup_trip_corr|corr_id|integer|Corridor identifier|
|matchup_trip_corr|gtfs_version|text|GTFS version identifier|
|matchup_trip_corr|route_id|integer|GTFS route identifier|
|matchup_trip_corr|trip_id|integer|GTFS trip identifier|
|matchup_trip_corr|shape_id|integer|GTFS shape identifier|


## <a id="intersections">Intersections</a>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|intersections|int_id|integer|Intersection identifier used to link other data to intersections|
|intersections|intersection_name|text|Name of intersection. Typically in {street1} and {street2} notation|
|intersections|intersection_lat|numeric|Intersection latitude|
|intersections|intersection_lon|numeric|Intersection longitude|

<br> </br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|metdat_intersections|int_id|integer|Intersection identifier|
|metdat_intersections|data_source|text|Data source type (e.g. traffic_counts)|
|metdat_intersections|source_id|text|Unique data source identifier (e.g. filename for traffic counts)|

## <a id="counts">Traffic Counts</a>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|traffic_counts|int_id|integer|Intersection identifier|
|traffic_counts|source_id|text|Data source identifier. In this case, it is the filename|
|traffic_counts|count_date|timestamp|Timestamp of counts|
|traffic_counts|zone_approach|text|Zone approach of counts (e.g. Eastbound)|
|traffic_counts|turn|text|Turn of counts (e.g. "S" for straight, "R" for right, etc.)|
|traffic_counts|volume|numeric|Traffic volume|
|traffic_counts|mode|text|Mode of transportation (e.g. auto, bike, ped, heavy)|

<br> </br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|metdat_traffic_counts|data_source|text|Data source type (i.e. traffic_counts)|
|metdat_traffic_counts|source_id|text|Unique data source identifier (i.e. filename)|
|metdat_traffic_counts|origin|text|Origin of counts (e.g. HDR, CoA)|
|metdat_traffic_counts|access|text|URL of file in online repository|
|metdat_traffic_counts|am_peak|integer|Binary variable denoting wether traffic counts include AM peak hours|
|metdat_traffic_counts|pm_peak|integer|Binary variable denoting wether traffic counts include PM peak hours|
|metdat_traffic_counts|off_peak|integer|Binary variable denoting wether traffic counts include off peak hours|
|metdat_traffic_counts|start_date|timestamp|Start date of traffic counts|
|metdat_traffic_counts|end_date|integer|End date of traffic counts|
|metdat_traffic_counts|autos|integer|Binary variable denoting wether traffic counts contain automobile counts|
|metdat_traffic_counts|heavy|integer|Binary variable denoting wether traffic counts contain heavy vehicle counts|
|metdat_traffic_counts|peds|integer|Binary variable denoting wether traffic counts contain pedestrian counts|
|metdat_traffic_counts|bike|integer|Binary variable denoting wether traffic counts contain bicycle counts|
|metdat_traffic_counts|bus|integer|Binary variable denoting wether traffic counts contain transit bus counts|
|metdat_traffic_counts|tmc|integer|Binary variable denoting wether traffic counts contain turn movement|
|metdat_traffic_counts|agg_level|numeric|Level of temporal aggregation in minutes (e.g. 15, 30, 60)|
|metdat_traffic_counts|raw|integer|Binary variable denoting wether traffic counts contain raw (disaggregate) counts|


## <a id="blue">Bluetooth</a>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|bt_sensor_sequence|id|integer|Sensor ID. References `bt_intersections`|
|bt_sensor_sequence|corr_id|integer|Corridor in which the sensor is located|
|bt_sensor_sequence|seq|integer|Order of consecutive sensors along the corridor|

<br></br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|bt_segments|corr_id|integer|Sensor ID. References `bt_intersections`|
|bt_segments|direction|text|Direction of travel. One of 'Southbound', 'Northbound', 'Eastbound' or 'Westbound'|
|bt_segments|segment|integer|Order of consecutive Bluetooth segments along the corridor|
|bt_segments|id_o|integer|Sensor ID of 'origin' location. References `bt_intersections`|
|bt_segments|id_d|integer|Sensor ID of 'destination' location. References `bt_intersections`|
|bt_segments|dir|text|One of 'Direction 1' or 'Direction -1'. Refers to `dir` column in `bt_daily_agg`|

<br></br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|bt_daily_agg|tod|text|Time of day in 'HH:MM' format at 15-minute intervals|
|bt_daily_agg|dir|text|One of 'Direction 1' or 'Direction -1'. Serves to distinguish between directions based on sensor sequence|
|bt_daily_agg|id_o|integer|Sensor ID of 'origin' location. References `bt_intersections`|
|bt_daily_agg|id_d|integer|Sensor ID of 'destination' location. References `bt_intersections`|
|bt_daily_agg|ttime_mean|numeric|Mean travel time between origin and destination sensors. Computed using methodology described above|
|bt_daily_agg|tt_sd|numeric|Standard deviation of travel time|
|bt_daily_agg|ttime_count|integer|Number of vehicles that inform the travel time estimation|
|bt_daily_agg|date|text|Date of observation in 'YYYY-mm-dd' format|
|bt_daily_agg|method|text|One of '2-point' or '4-point'. Describes the estimation procedure implemented to attain travel time value|
|bt_daily_agg|corr_id|integer|Corridor for which the estimation applies|

## <a id="grid">GRIDSMART</a>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|gs_intersections|id|integer|GRIDSMART camera ID|
|gs_intersections|street1|text|Main street of intersection where camera is located|
|gs_intersections|street2|text|Cross street of intersection where camera is located|
|gs_intersections|lat|numeric|Latitude of sensor location (SRID EPSG:4326)|
|gs_intersections|lon|numeric|Longitude of sensor location (SRID EPSG:4326)|

<br></br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|gs_movements|id|integer|GRIDSMART camera ID|
|gs_movements|intersection|integer|Intersection ID|
|gs_movements|guid|text|ID of pre-defined detection zone|
|gs_movements|zone_turn|text|Turns associated with detection zone|
|gs_movements|zone_approach|text|Approach associated with detection zone|

<br></br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|gs_agg_data|intersection|integer|Intersection ID. References `gs_movement`|
|gs_agg_data|timestamp|timestamptz|timestamp of counts in 15-minute intervals. Time zone is stored in UTC and should be converted to local time (US/Central)|
|gs_agg_data|zone_approach|text|Approach of counts. On of Northbound (NB), Southbound (SB), Eastbound (EB), Westbound (WB)|
|gs_agg_data|turn|text|Turn movement of counts. One of left (L), right (R), straight (S), U-turn (U)|
|gs_agg_data|heavy_vehicle|integer|0-1 variable if counts are for vehicles > 17 ft|
|gs_agg_data|volume|integer|Traffic counts at intersection|

## <a id="wave">Wavetronix (radar)</a>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|wt_intersections|id|text|Wavetronix sensor ID|
|wt_intersections|name|text|Name given to sensor|
|wt_intersections|int_geom_id|integer|Intersection ID where the sensors is located|

<br></br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|wt_data|id|text|Wavetronix sensor id. References `wt_intersections`|
|wt_data|timestamp|timestamp|Timestamp of counts in 15-minute intervals|
|wt_data|lane|text|Lane at which counts were detected|
|wt_data|direction|text|Direction at which counts were detected|
|wt_data|volume|integer|Traffic volume (mid-block) within 15-minute interval|
|wt_data|speed|numeric|Traffic average speed (mid-block) within 15-minute interval|

## <a id="gtfs">GTFS</a>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|gtfs_metadata|gtfs_version|text|Version identifier|
|gtfs_metadata|start_date|text|Version start date|
|gtfs_metadata|end_date|text|Version end date|

<br></br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|gtfs_routes|gtfs_version|text|GTFS version to which the data (row) corresponds to. See gtfs_metadata for specific version dates|
|gtfs_routes|route_id|integer|Route identifier|
|gtfs_routes|agency_id|integer|Agency identifier|
|gtfs_routes|route_short_name|text|Route name - abbreviated|
|gtfs_routes|route_long_name|text|Route name|

<br></br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|gtfs_trips|gtfs_version|text|GTFS version to which the data (row) corresponds to. See gtfs_metadata for specific version dates|
|gtfs_trips|route_id|integer|Route identifier|
|gtfs_trips|service_id|text|Identifies a set of dates when service is available for one or more routes|
|gtfs_trips|trip_id|integer|Trip identifier|
|gtfs_trips|trip_headsign|text|Contains the text that appears on signage that identifies the trip's destination to riders|
|gtfs_trips|direction_id|integer|Indicates the direction of travel for a trip. Use this field to distinguish between bi-directional trips with the same route_id|
|gtfs_trips|block_id|text|Identifies the block to which the trip belongs. A block consists of a single trip or many sequential trips made with the same vehicle|
|gtfs_trips|shape_id|integer|Defines a geospatial shape that describes the vehicle travel for a trip. See gtfs_shapes for geometry information|
|gtfs_trips|wheelchair_accessible|integer|Identifies whether wheelchair boardings are possible for the specified trip|
|gtfs_trips|bikes_allowed|integer|Identifies whether bicycles are allowed on the specified trip|
|gtfs_trips|dir_abbr|text|Direction abbreviation|

<br></br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|gtfs_stops|gtfs_version|text|GTFS version to which the data (row) corresponds to. See gtfs_metadata for specific version dates|
|gtfs_stops|stop_id|integer|Stop identifier. Serves to link to gtfs_trips table|
|gtfs_stops|stop_code|integer|Code of stop- redundant of stop id|
|gtfs_stops|stop_name|text|Name of stop in  street1/street2 format|
|gtfs_stops|stop_desc|text|Description of stop . Mostly provides stop name in street1 & street2 format|
|gtfs_stops|stop_lat|numeric|Stop location latitude|
|gtfs_stops|stop_lon|numeric|Stop location longitude|
|gtfs_stops|zone_id|text|Zone identifier with unknown mappings|
|gtfs_stops|wheelchair_boarding|integer|if wheelchair_boarding is allowed|
|gtfs_stops|corner_placement|text|Placement of stop with respect to corner|
|gtfs_stops|stop_position|text|Placement of stop with respect to block|
|gtfs_stops|on_street|text|Main street of stop location|
|gtfs_stops|at_street|text|Intersecting street of stop location|
|gtfs_stops|stop_point|text|Stop location point geometry in SRID EPSG:4326|

<br></br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|gtfs_stop_times|gtfs_version|text|GTFS version to which the data (row) corresponds to. See gtfs_metadata for specific version dates|
|gtfs_stop_times|trip_id|integer|Trip ID of transit vehicle. See gtfs_trips for more information|
|gtfs_stop_times|arrival_time|text|Scheduled trip arrival time|
|gtfs_stop_times|departure_time|text|Scheduled trip departure time|
|gtfs_stop_times|stop_id|integer|Stop identifier. Serves to link to gtfs_stops table|
|gtfs_stop_times|stop_sequence|integer|Order of stops along trip|

<br></br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|gtfs_shapes|gtfs_version|text|GTFS version to which the data (row) corresponds to. See gtfs_metadata for specific version dates|
|gtfs_shapes|shape_id|integer|Shape identifier. Serves to match shapes to trips. See gtfs_trips for shape IDs to trip IDs links|
|gtfs_shapes|shape_pt_lat|numeric|Point latitude|
|gtfs_shapes|shape_pt_lon|numeric|Point longitude|
|gtfs_shapes|shape_pt_sequence|integer|Point sequence|
|gtfs_shapes|shape_dist_traveled|numeric|Distance traveled in unknown units|

<br></br>
---

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
|gtfs_shapes_geom|gtfs_version|text|GTFS version to which the data (row) corresponds to. See gtfs_metadata for specific version dates|
|gtfs_shapes_geom|shape_id|text|Shape identifier. Serves to match shape geometries to trips. See gtfs_trips for shape IDs to trip IDs links|
|gtfs_shapes_geom|the_geom|geometry(Linestring, 4326)|Linestring geometry in SRID 4326|

## <a id="apc">Automated Passenger Counts (APC)</a>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
transit_apc|gtfs_version|text|GTFS time period for observation
transit_apc|trip_id|integer|Trip ID of vehicle
transit_apc|apc_timestamp|timestamp|timestamp of vehicle stop
transit_apc|route_id|integer|route ID of vehicle
transit_apc|veh_lon|numeric|longitude of transit vehicle location at time of logged data
transit_apc|veh_lat|numeric|latitude of transit vehicle location at time of logged data
transit_apc|ons|integer|number of vehicle boardings at stop
transit_apc|offs|integer|number of vehicle alightings at stop
transit_apc|actual_sequence|integer|place in vehicle stop sequence
transit_apc|max_load|integer|vehicle occupancy at stop
transit_apc|sched_time|timestamp|unclear - worth diving into
transit_apc|veh_id|integer|vehicle ID of the transit vehicle
transit_apc|direction_code_id|integer|code of direction of vehicle path - unclear on what it maps onto
transit_apc|dwell_time|integer|time the transit vehicle was at stop

## <a id="avl">Automated Vehicle Locations (AVL)</a>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
transit_avl|gtfs_version|text|GTFS version to which the data (row) corresponds to. See gtfs_metadata for specific version dates.
transit_avl|vehicle_id|integer|Vehicle ID of the transit vehicle
transit_avl|timestamp|timestamptz|Timestamp of logged data
transit_avl|speed|numeric|Speed of transit vehicle at time of logged data
transit_avl|route_id|integer|Route ID of transit vehicle. See gtfs_trips for more information.
transit_avl|trip_id|integer|Trip ID of transit vehicle. See gtfs_trips for more information.
transit_avl|lat|numeric|Latitude of transit vehicle location at time of logged data
transit_avl|lon|numeric|Longitude of transit vehicle location at time of logged data
