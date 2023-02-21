# Documentation for getting OS APIs in Power BI

## Introduction

This is the Getting Started Guide for using OS APIs within Power BI. There are a number of ways to get the OS APIs in Power BI and the best will depend on your own skills and goals. The options broadly fall into three categories: use a coding language such as R or Python, use power query within Power BI or use a custom visual from the visual store. Visuals are simply add-ons for Power BI that give you a custom visualisation or the ability to bring in other data types more easily. 
The APIs covered in this guide are the OS Maps API & OS Vector Tile API for the purposes of background mapping and the OS Places API. The following ways of integrating the APIs will be covered:

•	Mapbox Visual - a visual used to get the OS Vector Tile API integrated in Power BI as background mapping.

•	Icon Map - a visual used to get the OS Maps API integrated in Power BI as background mapping. 

•	R Custom Visuals - a method used to create a custom visual using OS Maps API and the programming language R.

•	ArcGIS Maps for Power BI - an ESRI visual using ArcGIS to integrate OS Maps API with power BI.

•	CadCorp - a CadCorp visual used to integrate OS APIs with Power BI.

•	Power query – use the built in power query functions to bring in and page OS places API.

•	Python – use a python script to bring in OS Places API.

•	R- use an R script to bring in OS Places API.


##Mapbox

Mapbox works with the OS Vector Tile API. First, bring in some data into Power BI or the visual will not work. Then, click on import visual and find the Map Box Visual:

![image](https://user-images.githubusercontent.com/97096971/220371965-b14c9a43-b2c4-4930-8aac-8ca6a5516052.png)

The Mapbox Visual will now appear in your build visual options tab. Click on it to bring in the visual and settings page. Add your longitude, latitude and location (this can be any field) and the visual will outline instructions on getting your Mapbox access token. You may need to set your latitude and longitude to ‘do not summarise’.

![image](https://user-images.githubusercontent.com/97096971/220372513-cce5d563-ef5d-44b3-9645-191f6c722574.png)

Navigate to the format visualisations tab and add in the access code. Change the Map Style to custom. Add in your OS Vector Tile API URL into the style and Mapbox API URL. An example of the URL is https://api.os.uk/maps/vector/v1/vts/resources/styles?key=[]&srs=27700. You will need to add in your own API key. This is obtained from the Data Hub 
(https://osdatahub.os.uk/docs/vts/overview). 

You will now have a visual with the OS Vector Tile API in your dashboard: 

