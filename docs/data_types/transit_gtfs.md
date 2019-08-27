# General Transit Feed Specification (GTFS)
General Transit Feed Specification (GTFS) is a way to describe a transit system network. It summarizes daily transit routes, stops and trajectories. The GTFS scheme, composed of 15 tables, is summarized [here](https://gtfs.org/reference/static). At CTR, we focus on 5 main tables and augment the information with 2 extra tables that provide metadata and shape geometries: `gtfs_routes`, `gtfs_trips`, `gtfs_stops`, `gtfs_stop_times`, `gtfs_shapes`, `gtfs_metadata` and `gtfs_shapes_geom`.

 Service periods for CapMetro are typically 3x a year: Jan through mid-June, mid-June through mid-August, and mid-August through the end of the year. CapMetro publishes a new GTFS scheme at the beginning of every service period in [TX data portal](https://data.texas.gov/browse?q=CapMetro%20GTFS&sortBy=relevance). APC and AVL data use the identifiers set in each service period GTFS description.

### Table Descriptions
`gtfs_metadata` s a versioning scheme developed at CTR to be able to distinguish between CapMetro varying versions of GTFS. It uses information from calendar_dates.txt table to infer service period dates.
`gtfs_routes` contains GTFS route information.
`gtfs_trips` contains GTFS trip information.
`gtfs_stops` contains GTFS stop information including point geometry.
`gtfs_stop_times` contains GTFS stop_times information.
`gtfs_shapes` contains GTFS shape information.
`gtfs_shapes_geom` contains GTFS shape geometry.

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
gtfs_metadata|gtfs_version|text| Version identifier
gtfs_metadata|start_date|text| Version start date
gtfs_metadata|end_date|text| Version end date

<br></br>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
gtfs_routes|gtfs_version|text|GTFS version to which the data (row) corresponds to. See gtfs_metadata for specific version dates.
gtfs_routes|route_id|integer|Route identifier.
gtfs_routes|agency_id|integer|Agency identifier.
gtfs_routes|route_short_name|text|Route name - abbreviated
gtfs_routes|route_long_name|text|Route name

<br></br>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
gtfs_trips|gtfs_version|text|GTFS version to which the data (row) corresponds to. See gtfs_metadata for specific version dates.
gtfs_trips|route_id|integer|Route identifier
gtfs_trips|service_id|text|Identifies a set of dates when service is available for one or more routes
gtfs_trips|trip_id|integer|Trip identifier
gtfs_trips|trip_headsign|text|Contains the text that appears on signage that identifies the trip's destination to riders
gtfs_trips|direction_id|integer|Indicates the direction of travel for a trip. Use this field to distinguish between bi-directional trips with the same route_id
gtfs_trips|block_id|text|Identifies the block to which the trip belongs. A block consists of a single trip or many sequential trips made with the same vehicle
gtfs_trips|shape_id|integer|Defines a geospatial shape that describes the vehicle travel for a trip. See gtfs_shapes for geometry information.
gtfs_trips|wheelchair_accessible|integer|Identifies whether wheelchair boardings are possible for the specified trip.
gtfs_trips|bikes_allowed|integer|Identifies whether bicycles are allowed on the specified trip.
gtfs_trips|dir_abbr|text|Direction abbreviation

<br></br>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
gtfs_stops|gtfs_version|text|GTFS version to which the data (row) corresponds to. See gtfs_metadata for specific version dates.
gtfs_stops|stop_id|integer|Stop identifier. Serves to link to gtfs_trips table
gtfs_stops|stop_code|integer|Code of stop- redundant of stop id
gtfs_stops|stop_name|text|Name of stop in  street1/street2 format
gtfs_stops|stop_desc|text|Description of stop . Mostly provides stop name in street1 & street2 format
gtfs_stops|stop_lat|numeric|Stop location latitude
gtfs_stops|stop_lon|numeric|Stop location longitude
gtfs_stops|zone_id|text|Zone identifier with unknown mappings
gtfs_stops|wheelchair_boarding|integer|if wheelchair_boarding is allowed
gtfs_stops|corner_placement|text|Placement of stop with respect to corner
gtfs_stops|stop_position|text|Placement of stop with respect to block
gtfs_stops|on_street|text|Main street of stop location
gtfs_stops|at_street|text|Intersecting street of stop location
gtfs_stops|stop_point|text|Stop location point geometry in SRID EPSG:4326

<br></br>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
gtfs_stop_times|gtfs_version|text|GTFS version to which the data (row) corresponds to. See gtfs_metadata for specific version dates.
gtfs_stop_times|trip_id|integer|Trip ID of transit vehicle. See gtfs_trips for more information.
gtfs_stop_times|arrival_time|text|Scheduled trip arrival time
gtfs_stop_times|departure_time|text|Scheduled trip departure time
gtfs_stop_times|stop_id|integer|Stop identifier. Serves to link to gtfs_stops table
gtfs_stop_times|stop_sequence|integer|Order of stops along trip

<br></br>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
gtfs_shapes|gtfs_version|text|GTFS version to which the data (row) corresponds to. See gtfs_metadata for specific version dates.
gtfs_shapes|shape_id|integer|Shape identifier. Serves to match shapes to trips. See gtfs_trips for shape IDs to trip IDs links
gtfs_shapes|shape_pt_lat|numeric|Point latitude
gtfs_shapes|shape_pt_lon|numeric|Point longitude
gtfs_shapes|shape_pt_sequence|integer|Point sequence
gtfs_shapes|shape_dist_traveled|numeric|Distance traveled in unknown units

<br></br>

|  **Table Name**  | **Field** | **Type** | **Description** |
|---|---|---|---|
gtfs_shapes_geom|gtfs_version|text|GTFS version to which the data (row) corresponds to. See gtfs_metadata for specific version dates.
gtfs_shapes_geom|shape_id|text|Shape identifier. Serves to match shape geometries to trips. See gtfs_trips for shape IDs to trip IDs links
gtfs_shapes_geom|the_geom|geometry(Linestring, 4326)|Linestring geometry in SRID 4326

### Data Considerations
There may be times when service period dates from the calendar_dates.txt table may overlap slightly. APC or AVL data may be used to distinguish the true date when the service periods changed.
