# AUTOMATED PASSENGER COUNTS (APC)
Automated Passenger Counts (APC) data is periodically published online at the [TX data portal](https://data.texas.gov/browse?q=APC&sortBy=relevance) at the end of service periods. The data contains information on boardings, alightings, occupancy and dwell time at every instance a transit vehicle makes a stop. The coordinates (lat/lons following spatial reference system EPSG:4326) are also recorded.

Service periods for CapMetro are typically 3x a year: Jan through mid-June, mid-June through mid-August, and mid-August through the end of the year. The route, trip, and vehicle IDs in the APC data should theoretically map onto General Transit Feed Specification (GTFS)<sup>[1](#f1)</sup> route, trip and vehicle IDs for the same period.

### Table Description

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

<br></br>

### Data Considerations

There are two key data considerations for the APC datasets:
* Some route and trip IDs do not map onto the GTFS IDs for the same service period (or any)
* Missing information takes the form of route or trip IDs that are recorded as 0

For example, for the first two service periods of 2017 (Jan 8th, 2017 - August 3rd, 2017)<sup>[2](#f2)</sup>, there were:
* 295009 transit vehicle stops with missing routes IDs recorded
* 40716 transit vehicle stops with missing trip IDs recorded
* 215550 transit vehicle stops with missing trip and route IDs recorded

In total, there were 551275 (6.15%) observations without identifying information. Further exploration is needed to get a sense of which routes or regions throughout the City of Austin suffer most from missing data.





<a name="f1">1</a>: [Click here for GTFS documentation](./transit_gtfs.md)</br>
<a name="f2">2</a>: From APC Data Exploration.html document
