# Using QGIS to create polygons with attributes that can be extruded in Mapbox Studio 
How to create more 3D-like maps

<!-- TOC -->

- [Using QGIS to create polygons with attributes that can be extruded in Mapbox Studio](#using-qgis-to-create-polygons-with-attributes-that-can-be-extruded-in-mapbox-studio)
    - [TL;DR](#tldr)
    - [Data](#data)
    - [Steps in QGIS](#steps-in-qgis)
    - [Steps in Mapbox](#steps-in-mapbox)
    - [Live example](#live-example)

<!-- /TOC -->

## TL;DR

Mapbox Studio has added extruded building polygons as of December 12. Let's make them anyway and see if we can do better?

## Data

Find the raster DEM and DSM for downtown Lexington and Shapefile of buildings with height attributes here: [outrageGIS.com/pointclouds/data/lex_building_heights_from_lidar.zip](https://outrageGIS.com/pointclouds/data/lex_building_heights_from_lidar.zip) (336 MB)

## Steps in QGIS

We need to add height information to building polygons. Unzip the data and add the digital elevation model, digital surface model, and building polygons.

![](images/b01.jpg)

The digital surface model (DSM) is height of all above-ground features, such as buildings, trees, etc. The digital elevation model (DEM) is the height of bare-earth ground. The raster datasets are created from Kentucky's lidar aerial mapping program. Check out our [Wildcat Eyes presentation](https://github.com/maptimelex/wildcat-eyes) to learn how to find and download this remarkable amount of data.
 

![](images/b02.jpg)   

Raster calculator to subtract bare-earth ground height from surface top to find height of above ground features.


![](images/b03.jpg)

Raster model of height of above-ground features.


![](images/b04.jpg)

Add polygons of building footprints.You find building polygons at Microsoft's [US Building Footprints](https://github.com/microsoft/USBuildingFootprints) repo. These polygons are from the impervious layer hosted on [Open Data Lexington](https://data.lexingtonky.gov/dataset/impervious_2007). Building footprints can also be found for selected areas on OSM.


![](images/b05.jpg)

You might need to edit polygons that overlap, e.g., two adjacent buildings with a great difference in height.


![](images/b06.jpg)

Create zonal statistics for the above-ground heights for each building polygon. Attributes will be added to the polygon layer that shows the average, highest, and lowest height of intersecting above-ground features.


![](images/b07.jpg)

Inspect the attribute table. If buildings are 1,000 ft high, then you might have some more editing to do. First, what happened? Some lidar data might have captured birds flying or a building polygon might not be correctly located and it is intersecting a radio tower. You'll need to inspect the high buildings and determine a proper value from neighboring pixels.

![](images/b09.jpg)

After editing building heights, a recognizable pattern should emerge.


![](images/b10.jpg)

Create a hillshade of the above-ground features. 


![](images/b11.jpg)

Export this layer as a GeoTIFF. We can add this to Mapbox Studio to add canopy top texture to our map.


![](images/b12.jpg)

Use QGIS 2.5D symbology method to preview effect in Mapbox.



![](images/b13.jpg)

Export building layer with minimal amount of attributes.

## Steps in Mapbox

![](images/b14.jpg)

Import GeoTIFF of DSM and building polygons.


![](images/b15.jpg)

Select buildiung layer and *Select data > Fill extrusion*. 

![](images/b16.jpg)

Convert building height to same units as Mapbox, i.e., meters.


![](images/b17.jpg)

Move raster layer beneath most of the vector layers and add opacity to raster layer.

## Live example

Check it out [live](https://api.mapbox.com/styles/v1/outragegis/ck43315k40umq1cootzplhdm5.html?fresh=true&title=view&access_token=pk.eyJ1Ijoib3V0cmFnZWdpcyIsImEiOiJjamY4ZWY1NXQyZWduMnFxN2M5cnB1YTJ6In0.BehfRddq9HyWFiy0mmGEbA)!