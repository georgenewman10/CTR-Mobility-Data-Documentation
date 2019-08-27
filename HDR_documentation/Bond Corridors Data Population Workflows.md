# Bond Corridors Database Workflows

Please note all path references are contained in X://NMC/Projects/HDR Corridors/code/bond_database_workflow

## Table of Contents
+ [Corridors](#corr)
+ [Bluetooth](#bt)
+ [GRIDMART](#gs)
+ [Wavetronix](#wt)
+ [GTFS](#gtfs)
+ [APC](#apc)
+ [AVL](#avl)
+ [Traffic Counts](#counts)

## <a name="corr">Corridors</a>

#### Table Creation

Corridors shapefiles are available in `/corridors/bond_corridors/`. The script `/corridors/corridor_direction.py` defines directionality for the corridors. To insert into the database, run `shp2pgsql -s 4326 -S bond_corridors public.corridors | psql -U vista -d bond_tool`

#### Corridor segmentation

Corridor segmentation is based on traffic signal locations. The script `/corridor/signal-corridor-segmentation.py` is used to a) match traffic signals to corridors, b) segment the corridors based on traffic signal locations and c) output the matched traffic signals and segmented corridors into shapefiles. `/corridors/corridor_traffic_signals` contains the matched traffic singnal shapefile information and `/corridors/corridor_segments` contains the corridor segment shapefile information. Please note that the code in `signal-corridor-segmentation.py` can be edited to match and segment corridors based on the locations of any type of sensors.

## <a name="bt">Bluetooth</a>

#### Table creation
The table creation statement is as follows:

`CREATE TABLE bt_daily_agg (tod text, dir text, id_o int, id_d int, ttime_mean numeric, tt_sd numeric, ttime_count int, date date, method text, corr_id int);`

#### Sensor Sequencing

Script `/bt/sensor_sequecing.R` identifies and orders bluetooth sensors for the Bond Corridors. It outputs a table "bt_sensor_sequence" in the database. If a table is already present, it drops it and creates a new one. I am under the impression that the code automatically creates the table (without a table definition). Also, I have recently edited it so that only important columns are saved.

#### Bluetooth daily aggregation

Script `bt\bt_aggregation.py` aggregates raw unmatched bluetooth records for sensors along corridors identified by `sensor_sequencing.R`. It populates the table `bt_daily_agg`.

#### Bluetooth segments

Script `/bt/bt_segments.py` uses the table `bt_sensor_sequence` in the database to create ordered bluetooth segments (paired bluetooth sensors in sequence). It outputs a table "bt_segments" in the database. The table has to already be created in order to populate it.

`CREATE TABLE bt_segments (corr_id int, direction text, segment int, id_o int, id_d int, dir text);`

Important things to note:
+ The code also provides direction (Southward, Northwards, etc.)
+ `id_o` and `id_d` are the bluetooth device IDs "_o" and "_d" stand for origin/destination

## <a name="gs">GRIDSMART</a>

#### Table creation

GRIDSMART information is stored in three tables: `gs_agg_data`, `gs_intersections`, `gs_movements`, and `gs_agg_data` holds GRIDSMART data aggregated to 15-minute intervals. `gs_intersections` ties intersection identifiers to camera identifiers so to tie devices to intersections. `gs_movements` defines the turn movements for each guid, which enables aggregation from raw data.

`CREATE TABLE gs_agg_data (int_id int, timestamp timestamptz, zone_approach text, heavy_vehicle int, volume int, speed_avg numeric, speed_std  numeric);`

`CREATE TABLE gs_intersections (id int, street1 text, street2 text, lat numeric, lon numeric, cam_id text, net_addr text);`

`CREATE TABLE gs_movements (id int, int_id int, guid text, zone_name text, zone_approach text);`

#### Data ingestion

Because the data is available through ATD Data Lake, Ken will develop data ingestion scripts based on cameras that have been identified to be along a bond corridor.

#### Data visualization

Script `/gs/zone_approach_plotting.R` is a rough script for plotting aggregated GRIDSMART data by approach and turn movement. `/gs/sharedstreets/` provides a rough framework to work with SharedStreets and plot GRIDSMART aggregated data based on SharedStreets references.

## <a name="wt">Wavetronix (radar)</a>

#### Table creation

Wavetronix information is stored in two tables: `wt_data`, and `wt_intersections`. The first holds the data, and the second ties detectors to intersections.

`CREATE TABLE wt_data (int_id int, det_name text, direction text, det_time timestamp, occupancy int, speed numeric, volume numeric);`

`CREATE TABLE wt_intersections (id int, int_name text, int_id int, lat numeric, lon numeric);`

#### Data ingestion

Because the data is available through ATD Data Lake, Ken will develop data ingestion scripts based on detectors that have been identified to be along a bond corridor.

#### Data visualization

The `hdr_corridor` application contains code to plot Wavetronix data.

## <a name="gtfs">GTFS</a>

#### Table Creation

Script `/transit/gtfs/gtfs_table_creation.sql` creates database tables for 8 GTFS tables, including the `gtfs_metadata` table. It does not ingest all GTFS tables, only the important ones.

The table `gtfs_metadata` is the following:

gtfs_version | start_date |  end_date  
--------------|------------|------------
 2016-1       | 2016-01-10 | 2016-06-04
 2016-2       | 2016-06-05 | 2016-08-20
 2016-3       | 2016-08-21 | 2017-01-07
 2017-1       | 2017-01-08 | 2017-06-03
 2017-2       | 2017-06-04 | 2017-08-19
 2017-3       | 2017-08-20 | 2018-01-06
 2018-1a      | 2018-01-07 | 2018-03-31
 2018-1b      | 2018-04-01 | 2018-06-02
 2018-2       | 2018-06-03 | 2018-08-25
 2018-3       | 2018-08-19 | 2019-01-05
 2019-1       | 2019-01-06 | 2019-06-15

All GTFS, AVL and APC data is augmented with a column `gtfs_version` as above depending on observation date.

#### Data ingestion

Actual GTFS data has to be manually downloaded from online sources. The data is in folders, and are named based on the GTFS version. Script `/transit/gtfs/gtfs_ingestion.py` provides a framework to ingest the data, including versioning scheme.

#### Added tables

Table `gtfs_shapes_geom` processes the table `gtfs_shapes` so to make shape geometries. This process is scripted in `/transit/gtfs/gtfs_shapes_geom.sql`.

#### Corridor specific information
Scripts to identify specific stops and trips relevant to specific corridors are in place: `GTFS Corridor Matchup-Second Method.py` and `trips_corridors_matchup.py`. They write to `matchup_stop_corr` and `matchup_trip_corr`, respectively. Below is the commands to create those tables:

`CREATE TABLE matchup_stop_corr (gtfs_version text, corr_id int, stop_id int, stop_dir int)`;

`CREATE TABLE matchup_trip_corr (gtfs_version text, corr_id int, route int, trip_id int, shape_id int)`;

## <a name="apc">APC</a>

#### Table creation

Script `/transit/apc/apc_table_creation.sql` gives the SQL statement to create the APC table. It only includes important columns. Plase note this has to be edited to include other columns that may have been missed previously. CapMetro recently updated their APC documentation to include *some* column definitions. These are available in `apc_fields_desc_from_online.text`.

#### APC ingestion

Script `/transit/apc/apc_data_calls.py` calls APC data from online sources, selects important columns and imports the data in the database. It also adds a `gtfs_version` column. This script needs to be updated to include the other columns are discussed above.

#### APC stop processing
Script `/transit/apc/apc_stop_times_processing.sql` processing GTFS and APC data to match APC observations to bus stops. This should be verified against the new columns `bs_id` that should be included when re-ingesting the APC data to the NMC-wide data repository.

#### Corridor specific information
To identify relevant APC data pertinent to specific corridors, call the table `apc_stop_matchup` in NMC-wide repository (currently DSTOP database) for the stops identified in `matchup_stop_corr` in the corridor-specific database.

## <a name="avl">AVL</a>

#### Table creation

Script `/transit/avl/avl_table_creation.sql` has a SQL table creation statement for AVL data. However, for ease of indexing, I created a column once the data was ingested based on the date part of the timestamp as a text column.

#### AVL ingestion

Script `/transit/avl/avl_ingest.py` ingests AVL data in a directory the database. It uses the date part of the data filename to understand which GTFS version it pertains to, and augments the data with a `gtfs_version` column

#### Corridor specific information
Script `/transit/avl/ingest_relevant_avl.py` ingests and processes AVL data. It ingests from NMC-wide repository (currently DSTOP database) and pre-processes the data for corridor speed computation. `/transit/avl/avl_speed_processing.R` then computes speed values per corridor and plots the information.

The following command will create the corridor-specific AVL table:


`CREATE TABLE matchup_avl_corr (corr_id int, trip_id text, timestamp timestamptz, frac_of_line numeric, corr_length numeric);`


## <a name="counts">Traffic Counts</a>

#### Table creation

Script `/traffic_counts/table_creation.sql` creates five tables: `intersections`, `matchup_int_corr`, `metdat_intersections`, , `metdat_traffic_counts` and `traffic_counts`. The table `intersections` outlines intersections pertinent to the corridors and provides coordinates, while `matchup_int_corr` ties the intersections to specific corridors. `metdat_intersections` links traffic count information to specific intersections. `metdat_traffic_counts` provides metadata on traffic counts, while `traffic_counts` hold the actual traffic count data.

#### Data ingestion

Ken has developed a PHP form that can be used to update the tables. It reads from CSV files that mirror the data model above and inserts only those records that have not been inserted previously.
