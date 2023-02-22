# Documentation for getting OS APIs in Power BI
This is the Getting Started Guide for using OS APIs within Power BI. There are a number of ways to get the OS APIs in Power BI and the best will depend on your own skills and goals. The options broadly fall into three categories: use a coding language such as R or Python, use power query within Power BI or use a custom visual from the visual store. Visuals are simply add-ons for Power BI that give you a custom visualisation or the ability to bring in other data types more easily. The APIs covered in this guide are the OS Maps API & OS Vector Tile API for the purposes of background mapping and the OS Places API. 

## Contents

The following ways of integrating the APIs will be covered:

•	Mapbox Visual - a visual used to get the OS Vector Tile API integrated in Power BI as background mapping.

•	Icon Map - a visual used to get the OS Maps API integrated in Power BI as background mapping. 

•	R Custom Visuals - a method used to create a custom visual using OS Maps API and the programming language R.

•	ArcGIS Maps for Power BI - an ESRI visual using ArcGIS to integrate OS Maps API with power BI.

•	CadCorp - a CadCorp visual used to integrate OS APIs with Power BI.

•	Power query – use the built in power query functions to bring in and page OS places API.

•	Python – use a python script to bring in OS Places API.

•	R- use an R script to bring in OS Places API.


## Mapbox

Mapbox works with the OS Vector Tile API. First, bring in some data into Power BI or the visual will not work. Then, click on import visual and find the Map Box Visual:

![image](https://user-images.githubusercontent.com/97096971/220371965-b14c9a43-b2c4-4930-8aac-8ca6a5516052.png)

The Mapbox Visual will now appear in your build visual options tab. Click on it to bring in the visual and settings page. Add your longitude, latitude and location (this can be any field) and the visual will outline instructions on getting your Mapbox access token. You may need to set your latitude and longitude to ‘do not summarise’.

![image](https://user-images.githubusercontent.com/97096971/220372513-cce5d563-ef5d-44b3-9645-191f6c722574.png)

Navigate to the format visualisations tab and add in the access code. Change the Map Style to custom. Add in your OS Vector Tile API URL into the style and Mapbox API URL. An example of the URL is https://api.os.uk/maps/vector/v1/vts/resources/styles?key=[]&srs=27700. You will need to add in your own API key. This is obtained from the Data Hub 
(https://osdatahub.os.uk/docs/vts/overview). 

You will now have a visual with the OS Vector Tile API in your dashboard.

## Icon Map

Icon map works with the OS Maps API. First, bring in some data into Power BI or the visual will not work. Then, click on import visual and find the Icon Map Visual:

![image](https://user-images.githubusercontent.com/97096971/220376778-44f5d161-f511-4ea8-858e-df6d26bd5277.png)

The Icon Map Visual will now appear in your build visual options tab. Click on it to bring in the visual and settings page. Add your longitude, latitude and category. Then navigate to the format visualisation tab and add in the OS Maps API URL in the WMS Layer Tab.  An example of the URL is https://api.os.uk/maps/raster/v1/zxy/Road_3857/{z}/{x}/{y}.png?key=xxxxxxxx. This is obtained from the Data Hub (https://osdatahub.os.uk/docs/wmts/overview). 

![image](https://user-images.githubusercontent.com/97096971/220376988-ec64dda8-92f1-4f9d-b3e0-c4a6cdc09db3.png)

You will now have a visual with the OS Maps API in your dashboard: 

![image](https://user-images.githubusercontent.com/97096971/220377085-62fd9a2f-de1c-410a-b76b-7ea1d745ab87.png)

## Custom visuals using R 

Power BI supports both R and Python. R has the ability to create a custom, interactive html visual. This section will run through the steps to achieve this. The data required is in the ' R custom visual files' folder. It will require the installation of node.js and will produce a html visual that is customisable with R, offering varying degrees of interactivity within powerBI. The following method was created by the Welsh Ambulance Service NHS Trust. 

Install the JavaScript server node.js from here. This will require administrator permissions on your machine. 

Once installed, launch cmd prompt and navigate to a new folder (e.g. cd C:\Users\\OneDrive - Ordnance Survey\PowerBI). From this location run this command in cmd prompt in your project directory (npm install -g powerbi-visuals-tools). 

Check your installation by typing pbiviz within command line, which should return information on your install.

![image](https://user-images.githubusercontent.com/97096971/220654417-60f87d4a-1f71-49f1-a89c-c91c87dae308.png)

Navigate to your desired working directory within command line (e.g cd C:\Users\obowden\OneDrive - Ordnance Survey\Documents\PowerBI¬). From here create a new rhtml template with the command pbiviz new testproject -t rhtml.

![image](https://user-images.githubusercontent.com/97096971/220654518-3d652a4d-0b5e-4ee7-9527-ac3e5cf27ef3.png)

Within the newly created project folder, open the pbviz.json file in the template folder and populate the author, description, support URL & GitHub URL. If you do not have this information populate the fields with a dash, as in the below example.

<code>{"visual":{"name":"test","displayName":"R7","guid":"r760E3C7E163EC4E6182208A920885DDFF","visualClassName":"Visual","version":"1.0.0","description":"-","supportUrl":"-","gitHubUrl":"-"},"apiVersion":"3.8.0","author":{"name":"-","email":"-"},"assets":{"icon":"assets/icon.png"},"externalJS":null,"style":"style/visual.less","capabilities":"capabilities.json","dependencies":"dependencies.json","stringResources":[]}
</code>.

The script source file can be altered to use different R scripts and create different visuals. An example developed by the Welsh Ambulance Service NHS Trust of a script that brings in OS Maps API and allows for switching between different styles. Update the script with your OS Maps API key obtained via the OS Data Hub and dataset with X Y coordinates, then and overwrite the contents of the script file within your project folder to test this.

    # Install packages

    library(XML)
    library(leaflet)
    library(leaflet.extras)
    library(htmlwidgets)
    library(htmltools)
    library(magrittr)
    library(sf)
    library(rgdal)

    # Get working directory
    my_dir <- getwd()

    # Import data from csv

    dataset  <- read.csv("C:/Users/obowden/OneDrive - Ordnance Survey/PowerBI/R/sampleRHTMLVis/wmc_hospital_locations_2021_11_18.csv")

    # Create a list of labels

    marker_labels <- lapply(seq(nrow(dataset)),
    function(i) {paste0( '<p>',
    "Name: ",dataset[i, "Name"], '<br>',    #Name of hospital taken from Name column
    "Type: ",dataset[i, "Service"], '</p>' )#Type of hospital taken from Service column
    })
    marker_icon <-  icons(iconUrl = ifelse(dataset$Service == "Major A&E",#Create a list of icons dependant on the Service column
    "C:/Users/obowden/OneDrive - Ordnance Survey/PowerBI/R/sampleRHTMLVis/hospital-fill.png",
    "C:/Users/obowden/OneDrive - Ordnance Survey/PowerBI/R/sampleRHTMLVis/hospital-line.png"),
    iconWidth = 20, iconHeight = 20)

    # API key for Ordnance Survey maps

    Sys.setenv(OS_PROJECT_API_KEY = 'Insert key here')

    # Define url templates for the different tile styles

    OSRoad_3857     <- paste0("https://api.os.uk/maps/raster/v1/zxy/Road_3857/{z}/{x}/{y}.png?key=",    Sys.getenv("OS_PROJECT_API_KEY"))
    OSLight_3857    <- paste0("https://api.os.uk/maps/raster/v1/zxy/Light_3857/{z}/{x}/{y}.png?key=",   Sys.getenv("OS_PROJECT_API_KEY"))
    OSOutdoor_3857  <- paste0("https://api.os.uk/maps/raster/v1/zxy/Outdoor_3857/{z}/{x}/{y}.png?key=", Sys.getenv("OS_PROJECT_API_KEY"))

    # Attribution for map tiles
    OS_Attribution  <- "© Crown copyright and database rights 2023"

    # Create map in leaflet
    m <- leaflet(dataset) %>%
    addTiles(OSRoad_3857,
    attribution = OS_Attribution,
    group ='OS Road') %>%
    addTiles(OSLight_3857,
    attribution = OS_Attribution,
    group ='OS Light') %>%
    addTiles(OSOutdoor_3857,
    attribution = OS_Attribution,
    group ='OS Outdoor') %>%
    addMarkers(lng    = ~Longitude,                            
    lat    = ~Latitude, 

    # Label with information that appears when clicking on icon
    label  = lapply(marker_labels, htmltools::HTML),

    # Label with information when you hover over icon
    popup  = lapply(marker_labels, htmltools::HTML),

    # Icon displayed dependant on Service column
    icon   = marker_icon,                           
    group  = "Icons") %>%
    addCircleMarkers(lng    = ~Longitude,                            
    lat    = ~Latitude, 

    # Radius of circle                           
    radius = 10,    

    # Label with information that appears when clicking on circle
    label  = lapply(marker_labels, htmltools::HTML),

    # Label with information when you hover over circle
    popup  = lapply(marker_labels, htmltools::HTML),

    # Circle colour dependant on value of Service column
    color  = ifelse(dataset$Service == "Major A&E", "red","orange"),
    group  = "Circles") %>%
    addMarkers(lng   = ~Longitude,                            
    lat   = ~Latitude,    

    # Label with information that appears when clicking on marker
    label = lapply(marker_labels, htmltools::HTML),

    # Label with information when you hover over marker
    popup = lapply(marker_labels, htmltools::HTML),
    group = "Markers") %>%
    addLayersControl(baseGroups    = c("OS Road","OS Light","OS Outdoor"),
    overlayGroups = c("Icons","Circles","Markers"),
    position      = "bottomleft",
    options       = layersControlOptions(collapsed = FALSE)) %>%
    hideGroup("Circles")%>%

    # Hides the markers so that it just defaults to circles
    hideGroup("Markers") 
    m
 
Navigate to the new project folder and run (pbiviz package). 

![image](https://user-images.githubusercontent.com/97096971/220666334-0b094f38-69da-473f-9aa9-9b3ce01ca53f.png)

This will create a custom visual within the dist folder of your project. Import this to PowerBI by selecting ‘Import’ and ‘Power BI Visual from File’ (you will have need to also go into the Power BI options and turn the R scripting on). The visual will then appear as an option in your visualisation tab.

![image](https://user-images.githubusercontent.com/97096971/220667371-5901fd5f-38bc-46dd-aacb-17eb9c6ab553.png)

![image](https://user-images.githubusercontent.com/97096971/220679940-f7be2021-b9c6-47c0-80aa-faa74ca6bfe8.png)

More documentation can be found in the documentation saved in the R custom visual folder.
