---
output:
  html_document:
    theme: yeti
    keep_md: true
  word_document: default
params:
  set_title: "Work Zone Traffic Information Web App Tutorial"
title: "Work Zone Traffic Information Web App Tutorial"
---



# {.tabset .tabset-fade}

## Initial Setup
You can access the web app [here](https://nmc-shiny.ctr.utexas.edu/LUC_fixed_sensors/)


### Step 1: Select a study area
  - Choose to study data from all available corridors, or a corridor sub-section.
  - To analyze a corridor sub-section: first select a point on the map, then click the **Select** button below **Start Sensor**. Repeat this process below **End Sensor** to complete the process.

<center>![Step 1](./screenshots/step_1.png){width=300px} </center>

### Step 2: Select a time period
  - With the **Historical time** option selected, clicking on the dates below **Start** and **End**	will reveal a calendar to quickly choose starting and ending dates. Alternatively, you can
	leave the **Last 72 hours** option selected to study the most recent data. When you've selected your desired range, click **Retrieve Closure Data**.
	  - Note: We recommend keeping this range as small as possible, in order to retrieve the data more quickly.

<center>![Step 2](./screenshots/step_2.png){width=300px} </center>

### Step 3: Select a reference time period to compare against
  - We have found the **Default time range** option to provide a good baseline to test against, but feel free to select a different range. Click **Retrieve Historical Data** once you've
	selected your desired range.

<center>![Step 3](./screenshots/step_3.png){width=300px} </center>

### Step 4: Choose analysis parameters
  - The **Queue Speed Threshold** drop down box allows you to choose what speed constitutes a 
	queue. 
    - For example: given a 20 mph threshold, the app defines a queue as any traffic moving less than 20 mph.
  - Click **Create Plot** once you've set your parameters.
  - Click the **Output: Delay** and **Output: Impacted Vehicles** tabs at the top of the page to view your results.
  
<center>![Step 4](./screenshots/step_4.png){width=300px} </center>

### Step 5: View your results
  - Navigate to the **Output: Delay** and **Output: Impacted Vehicles** tabs to view the results of your analysis. 
  - The **Interpreting Results** tab of this tutorial page can be used to better understand what the resulting graphs represent. </br></br></br></br>



## Interpreting Results

### Queue Length
  - The queue length graph displays the length of a queue (measured in miles) as time progresses. The queue speed threshold you defined in step 4 will determine the amount of data displayed here. As the threshold increases, more data will be included in this plot. 

<center>![Queue Length](./screenshots/interpret_1.png){width=900px} </center></br>

### Queue Position
  - The queue position graph shows a more complete picture of queue formation, in that you can see the exact recorded speed at each sensor within your study area. To do this, simply hover your mouse over a data point. 

<center>![Queue Position](./screenshots/interpret_2.png){width=900px} </center></br>

### Travel Time
  - The travel time graph compares the closure date travel time (from step 2) to the reference travel time (step 3).   

<center>![Travel Time](./screenshots/interpret_3.png){width=900px} </center></br>

### Delay Time
  - The delay time graph shows the time delay per vehicle for the study period.
  
<center>![Delay Time](./screenshots/interpret_4.png){width=900px} </center></br>

### Speed Distribution
  - Hover your mouse over any part of the pie chart to see the exact number of vehicles moving in each 5 mph window. 
  
<center>![Speed Distribution](./screenshots/interpret_5.png){width=500px} </center></br>

### Affected Vehicles by Speed Distribution
  - This graph displays the number of vehicles experiencing each level of delay, based on their recorded speed. In the example below, 3,501 vehicles experienced a 2 minute delay, due to traveling in the 25-30 mph range. 
  
<center>![Affected Vehicles by Speed Distribution](./screenshots/interpret_6.png){width=900px} </center></br>

### Total Delay by Speed Distribution 
  - This graph shows the total time spent traveling at each speed. When comparing to the previous graph, notice how the values from the lower speed windows undergo a serious increase. In the example below, 7,638 minutes were spent traveling in the 25-30 mph range.

<center>![Total Delay by Speed Distribution](./screenshots/interpret_7.png){width=900px} </center></br>

### Detected Vehicles
  - This shows the number of vehicles detected during the analysis, compared to a typical scenario.

<center>![Detected Vehicles](./screenshots/interpret_8.png){width=900px} </center></br>


## Closure Time Planning


### Setup
  - Navigate back to the **Analysis Setup** tab. From there, first select the corridor sub-section you wish to study, just as you did in step 1. Next, select a reference time period, as you did in step 3. Once your reference time period has been selected, click the **Closure Plan** button. 
    - Note: We recommend selecting a sub-section because it will ensure more consistent results. Likewise, when selecting your reference time period, make sure you select a period of at least one week. Unfortunately, the data contains some minor gaps. Because of this, trying to analyze all the corridors at once, or selecting only a few days as a reference period will force the app to search for data that doesn't exist, causing crashes. 
  - With a corridor sub-section and reference time period selected, navigate to the **Closure Time Planning** tab to view your results.
  - After selecting your closure starting and ending times, select a time range. Choosing a small time range will display a very specific hour by hour breakdown. A larger time range will give you more insight as to the average and standard deviation over longer periods of time.

### Results
  - These graphs display the average number of vehicles passing the selected sensor during the reference time period.

<center>![Closure Planning Bar Chart](./screenshots/closure_bar.png){width=1100px} </center></br>
  
<center>![Closure Planning Line Graph](./screenshots/closure_line.png){width=1100px} </center></br>
