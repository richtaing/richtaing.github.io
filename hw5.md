---
layout: page
title: Homework 5
permalink: /hw5/
---

<div id="dashboard"></div>
<script type="text/javascript">
  var spec = "assets/json/dashboard.json";  // Make sure this path is correct!
  vegaEmbed('#dashboard', spec).then(function(result) {
  }).catch(console.error);
</script>

Visualization 1: Geographic Density (STATIC)
For the first visualization, I created a static density heatmap to represent the geographic distribution of Bigfoot reports across the United States. Instead of plotting individual points, which often results in overplotting and obscures patterns in large datasets, this view aggregates the data into a grid. This provides a clear, high-level overview of where sightings are most concentrated without needing user input.

- Encoding Types: I used the rect mark to construct the heatmap. Latitude and Longitude are mapped to the Y and X axes respectively.
- Color Scheme: I utilized the inferno color scheme to encode the count() of reports within each grid cell. I chose this sequential palette (dark to bright) because it is perceptually uniform and draws the eye immediately to "hotspots" (bright yellow/orange) against the darker background, effectively communicating density differences.

I filtered the dataset in Python to remove rows with missing geographic coordinates (NaN in latitude/longitude). Additionally, I used Altair's internal binning transformation (bin=alt.Bin(maxbins=60)) on the coordinates to transform the raw, scattered data points into the structured grid format seen in the plot.


Visualization 2: Seasonal Breakdown (Added Interaction)
For the second visualization, I built a stacked bar chart that adds a layer of interactivity to the first plot. While the map shows where reports happen, this chart explains when they happen. I linked this chart to the map so that it dynamically updates based on the user's selection, effectively turning the static map into an interactive exploration tool.

- Encoding Types: I used a bar mark with state on the Y-axis and count() on the X-axis.
- Color Scheme: I colored the bars by season using a nominal color palette. This "stacked" design allows users to see the seasonal composition of reports for any given state.

For interactivity, I added a brushing and linking interaction to connect the two plots. This allows users to click and drag on the map (Visualization 1) to select a specific geographic region, which then filters Visualization 2 to show only the states included in that selection. User can then access specific clusters (like the Pacific Northwest) and see the specific seasonal breakdown for that area.
I implemented a "cross-filter" transformation, which allows the barchart to accept the filter from the map's brush (transform_filter(brush)). The interactivity works hierarchically: the Season Dropdown filters the entire dataset first (updating the map), and the Map Brush then filters those results spatially. This ensures that the "Seasonal Breakdown" chart always reflects exactly what is visible on the map and shows the specific statistics for the selected season and region.
