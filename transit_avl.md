# Automated Vehicle Location (AVL)
Automated Vehicle Location data reports transit vehicle location coordinates (following spatial reference system EPSG:4326), speed and timestamp of record among other identifying information for transit vehicles equipped with technology. It has been historically broadcasted by CapMetro every 2 minutes. As of Summer 2019, CapMetro increased the rate of broadcasting to 30 seconds.

### Table Description

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

<br></br>

### Data Considerations
* Some route and trip IDs do not map onto the GTFS IDs for the same service period.
* Corridor transit speed can be computed by processing the data as outlined in "AVL Transit Speed Documentation.html"
