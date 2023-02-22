# Documentation for getting OS APIs in Power BI

There are a number of ways to get the OS APIs in Power BI and the best will depend on your own skills and goals. The options broadly fall into three categories: use a coding language such as R or Python, use power query within Power BI or use a custom visual from the visual store. Visuals are simply add-ons for Power BI that give you a custom visualisation or the ability to bring in other data types more easily. 

The APIs covered in this guide are the OS Maps API & OS Vector Tile API for the purposes of background mapping and the OS Places API. 

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

Power BI supports both R and Python. R has the ability to create a custom, interactive html visual. This section will run through the steps to achieve this. The data required is in the ' R custom visual files' folder. It will require the installation of node.js and will produce a html visual that is customisable with R, offering varying degrees of interactivity within powerBI. The following method was created by the Welsh Ambulance Service NHS Trust. Further documentation from NHS Wales Modelling Collaberative can be found [here(https://github.com/OJOllie/Power-BI/blob/main/R%20Custom%20Visual%20Data/Documentation.Md).

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

# ArcGIS Maps for Power BI 

ArcGIS Maps for Power BI works with APIs due to its link to ArcGIS Online (AGOL). You will need an ESRI enterprise licence or an AGOL account to access this. Maps you have within your AGOL account can be imported into Power BI. This includes basemapping from OS Maps API if your organisation has set these up as an option with your enterprise account. 

Add in an ArcGIS Maps for Power BI visualization. 

![image](https://user-images.githubusercontent.com/97096971/220687809-e12b5918-a012-4bfe-8787-fa6238b5abdc.png)

This will display a sign in portal where you need to input your credentials.

![image](https://user-images.githubusercontent.com/97096971/220687865-2ed830fc-8cc0-4ac8-add9-d15b1ef36f8e.png)

Once this is done you will be able to bring in basemaps and content from your account, including OS APIs you have as basemaps within AGOL

![image](https://user-images.githubusercontent.com/97096971/220687923-3288e9db-85fd-4c9c-bc54-4bf49909c8e4.png)

# Cadcorp SIS WebMap for Power BI

The CadCorp custom visual can be used with the CadCorp SIS Webmap software to create pull in OS APIs. The data from your webmap can be pulled in using the custom visual, including the OS APIs. Information on adding the APIs to your webmap can be found here https://help.cadcorp.com/en/9.0/webmapadmin/layers-osdatahub-wmts.htm?Highlight=OS%20maps%20api. 

To show the webmap in to Power BI click on import a visual from a file in the visualisation pane.

![image](https://user-images.githubusercontent.com/97096971/220698540-ba2c9965-ea3f-42d4-a5b8-8df6741231fe.png)

Point it to your CadCorp SIS Webmap for Power BI visual. This will appear in your visual tab. Bring this in to your report and add X Y data to activate the visual. Follow the instructions in the visual.

![image](https://user-images.githubusercontent.com/97096971/220698591-3f3b0090-3949-47df-81d1-8156d5bdf712.png)

After adding your in service URL for your webmap, the map will appear in the report with any OS APIs you have included.

# Power Query- OS Places API

There are multiple ways to bring OS Places API into Power BI. Each way must be able to page through the API results or only the first 100 features will be returned by the API call. This section will run through how to get OS Places API in Power BI using power query. 

Bring in the data using your API URL. Include the offset parameter in your URL so it can be parameterised by Power Query at a later stage:

https://api.os.uk/search/places/v1/postcode?postcode=CF11&offset=0

To do this use the get data option and choose web:

![image](https://user-images.githubusercontent.com/97096971/220698835-3e6185c6-2f3b-45f9-bdcc-b2f892a0f8e3.png)

Choose the advanced option. Include your API key as a header with ‘key’ as the header request:

![image](https://user-images.githubusercontent.com/97096971/220698863-f21a7626-fb04-4643-be59-439de4bd2323.png)

This will pull the first 100 results of the API call into Power Query. From here we want to parameterise the offset part of the URL. Under the home tab choose manage parameters and select new. 

![image](https://user-images.githubusercontent.com/97096971/220698892-df1df14c-17c3-4871-b006-e48aac11019e.png)

Call the new parameter offset, give it a type of text and set the current value to zero.

![image](https://user-images.githubusercontent.com/97096971/220698940-0013fda6-0436-4a0f-ba64-dd3dba6ee161.png)

Your offset parameter will now appear in other queries. Next, navigate to the query that is calling and processing the OS Places API, click the view tab and choose advanced editor.

![image](https://user-images.githubusercontent.com/97096971/220698988-ae11b647-68e1-4562-85a4-bc80ebe3f489.png)

Include the offset parameter in the URL. As the current value is set to one the query should run as normal for now.

![image](https://user-images.githubusercontent.com/97096971/220699027-562ea2d8-8007-4747-a156-bc11b563bb39.png)

To be able to change the parameter, change the whole query to a function. Right click your query and choose create function, call it fnpaging or similar. 

![image](https://user-images.githubusercontent.com/97096971/220699054-30e4c828-04d5-4c92-8a8c-5d9ec4d53e50.png)

The function can be invoked by changing the offset parameter and clicking invoke. For example if we change the offset value to 100 the next 100 results will appear as a new query.

![image](https://user-images.githubusercontent.com/97096971/220699101-8626be0a-536a-495d-821c-c96c182c7e4d.png)

The function can also be called against a column with offset values (e.g 100, 200, 300, 400) to pull back all the results in one go. But for now, we will invoke the function three times with values 100, 200, 300 and 400 and rename the tables to something sensible. 

![image](https://user-images.githubusercontent.com/97096971/220699134-9e89ce31-ce6d-44e0-aead-85c2db5010b3.png)

Next, under the home tab choose append queries as new.

![image](https://user-images.githubusercontent.com/97096971/220699166-a85bcca1-f25d-4592-892a-173e61031270.png)

Select all of your offset queries and append them together.

![image](https://user-images.githubusercontent.com/97096971/220699207-7c2a0d8b-04c3-4433-b0ac-defac99d8910.png)

Rename the appended query to ‘results’. In this case, the results table contains the first 500 features returned by the OS Places API for a particular postcode. More offset queries can be added to increase this amount. 

# Python- OS Places API

Choose get data and more. Search python script and select that option.

![image](https://user-images.githubusercontent.com/97096971/220699631-70b81c79-199c-45cd-98a9-18efdd0c46d6.png)

Paste the following script into the window, substituting your API key and your chosen parameters for the OS Places API call:

    # Import packages
    import pandas as pd
    import requests as r

    # Define API parameters
    key = 'enter API key here'
    query = 'enter OS Places query here (e.g EX4)'
    url = f'https://api.os.uk/search/places/v1/find?query={query}&key={key}'
    dataframe = pd.DataFrame()

    # Make API requests with offsets and concatenate results
    offsets = range(0, 1000, 100)
    for offset in offsets:
        url = f'https://api.os.uk/search/places/v1/find?query={query}&offset={offset}&key={key}'
        response = r.get(url)
        response.raise_for_status()  # Check for errors
        results = response.json()['results']
        page = pd.json_normalize(results)
        dataframe = pd.concat([dataframe, page])
        
    # Delete page so only main dataframe is detected by PowerBI 
    del page

Load in the dataframe.

![image](https://user-images.githubusercontent.com/97096971/220700729-b3ed2b77-d529-4cd0-b7bb-a4281148fc96.png)

This will have loaded in the first 1000 results from the API call. The number of results can be adjusted by changing the maximum range value in the script.

# R- OS Places API

Choose get data and more. Search R script and select that option.

![image](https://user-images.githubusercontent.com/97096971/220700859-1328c4ab-3ec7-49b6-adf0-5f2105699628.png)

Paste the following script into the window, substituting your API key and your chosen parameters for the OS Places API call:

    # Import packages
    library("httr")
    library("jsonlite") 
    library('gtools')

    # Define API parameters
    key <- 'FzR8BJRJvs5f1w0FaGOQk5mdwQrOwBhe'
    query <- 'CF11'

    # Define empty dataframe and offset. Change second value to increase number of results.
    offset= seq(0, 900, by=100)
    dataframe = data.frame()

    # Make API requests with offsets and concatenate results
    for (value in offset) {
      url <- paste0('https://api.os.uk/search/places/v1/find?query=',query,'&offset=',value)
      data <- GET(url,add_headers(key=key))
      json <- fromJSON(rawToChar(data$content), flatten=TRUE) 
      dataframe <- smartbind(dataframe,json$results)
    }

![image](https://user-images.githubusercontent.com/97096971/220700960-e89ac77a-3af4-49e5-9183-86e7d6a91fc8.png)

This will have loaded in the first 1000 results from the API call. The number of results can be adjusted by changing the maximum range value for offset in the script.


