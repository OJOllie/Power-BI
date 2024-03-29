# R visualisations in PowerBI
### WMC hackathon group 5
### 19/11/2021

![image](https://user-images.githubusercontent.com/97096971/220682372-50dfbef7-f104-40bf-b4bc-a6730babdbe7.png)

# What this document will show you

This document will show you how to set up an R custom visual in PowerBI and will tell you the prerequisites for this to be done.A summary of the requirements and limitations of creating R visuals in PowerBI can be found here, for easy access. Other resources are noted through the document and compiled in Appendix 1.

Please feel free to edit this with any trouble shooting notes with resolutions to common issues.

# Prerequisites

You need to be or have the help of the admin of your PC to be able to complete steps 1-5, the inital set up.

PowerBI currently runs on Microsoft R version 3.4.4. This can be downloaded here and a list of packages in use within this version can be found [here](https://cran.microsoft.com/snapshot/2018-04-01/web/packages/available_packages_by_name.html)

# Initial set up to allow creation of a PowerBI R visual

Steps 1-5 all require administrator permissions so be sure to raise a ticket with your local IT as soon as you know you will need to use PowerBI in combination with R.

Information for this section of the document was taken from: https://radacad.com/interactive-map-using-r-and-power-bi-create-custom-visual-part-1

## Step 1

Install node.js (which essentially is javascript server) from [here]: (https://nodejs.org/en).

## Step 2

Launch command prompt or power shell
Step 3
Navigate to your desired working directory (create a new folder where you want to store the custom visuals if needed)
e.g.  <code>cd C:\Users\be234789\Documents\RCustomvis</code>

## Step 4
Install the powerbi-visuals-tools package with Powershell
run: <code>npm install -g powerbi-visuals-tools</code>

## Step 5
Create an rhtml template
run: <code>pbiviz new fredTimelineVis -t rhtml</code>

# Set up each time you create a PowerBI R visual

## Step 6
Navigate to the folder where template is stored

e.g. <code>cd C:\Users\be234789\Documents\RCustomVis\sampleRHTMLVisual</code>

## Step 7
This is an important step for compilation

open the pbviz.json file in the sampleRHTMLVisual folder

edit to add author, description, and support URL & gitHub URL (can be fake urls) to the pbviz.json and then save:

e.g. `…

        "description":"Horizontal timeline visual build using ggplot and plotly in R",
        
        "supportUrl":"https://medium.com/@fredtasker/interactive-power-bi-custom-visuals-with-r-a6a4ac998710",
        
        "gitHubUrl":"https://github.com/fredwise/PM_timeline"},
        
        ...
        
        "author":{"name":"Fred Tasker","email":"xxxx.com"}
        
        ...'

## Step 8

Go back to command prompt/power shell and run the following to compile (make sure working directory sill set to where R script file located)

e.g. <code>cd C:\Users\be234789\Documents\RCustomVis\sampleRHTMLVisual</code>

## Step 9

Run: <code>pbiviz package</code>

## Test this has worked

Once successfully compiled the custom PBI visual will now be ready, you can now import the Microsoft Power BI Custom visual file into PBIto test file found in folder called “dist” in sampleRHTMLVisual folder, will be a .pbiviz file
to import a custom visual into PBI go to the additional options in the visualizations pane (…) and select ‘import a visual from a file’.

## Customing your visual

Now that you have the example template you can customise accordingly: * alter the script.R file (will need to retain some elements: the first line"source('./r_files/flatten_HTML.r')", libraryRequireInstall() and internalSaveWidget() arguments) * add any cran package dependencies from the script to dependencies.json * edit the name of the visual, as well as the icon in pbiviz.json * to change the PBI parameters that feed into the visual - edit the capabilities.json file accordingly (add/remove additional data roles)

Notes on how to do this can be found [here](https://radacad.com/interactive-charts-using-r-and-power-bi-create-custom-visual-part-2).

## Tip for creating R visuals in PowerBI
Microsoft keep a log of the [Microsoft R (MRAN) release history](https://mran.microsoft.com/release-history), this allows install of past MRAN versions. This makes it easier to test visuals.

You can still use the most recent version of Microsoft R or R for some things, and MRAN 3.4.4 for visualisations that will go into PowerBI by changing the global options within RStudio

![image](https://user-images.githubusercontent.com/97096971/220686903-24302c7b-ec89-49a4-ba6d-bb029a273833.png)

Global options can be found in the Tools tab

![image](https://user-images.githubusercontent.com/97096971/220686936-c1d8ff19-70ee-4a39-a4bb-4adcbb840ee7.png)

You can change the R version you are using

![image](https://user-images.githubusercontent.com/97096971/220686976-483c937d-5cc8-4db5-93c5-8df683ece593.png)

Pick any of your downloaded R versions to use in the session

## More information

https://radacad.com/create-custom-visual-with-r-and-json-part3

https://radacad.com/create-custom-visual-with-r-and-json-part-4

## Appendix 1 - resources mentioned and used in this document

Microsoft essential information on creating visuals made using R packages in PowerBI:

https://docs.microsoft.com/en-us/power-bi/connect-data/service-r-packages-support

Microsoft R (MRAN) release history, allows install of past MRAN versions. This makes it easier to test visuals, as you can download this version and use it to test visuals by swapping R version in use in your global options (mentioned in the tips section):

https://mran.microsoft.com/release-history

Microsoft has a ‘time machine’ which allows you to examine the different MRAN releases and look at the information at the time of release:

https://mran.microsoft.com/timemachine

Specific packages in CRAN when MRAN 3.4.4 was uploaded (2018-14-01):

https://cran.microsoft.com/snapshot/2018-04-01/web/packages/available_packages_by_name.html

Where the process we have outlined comes from - a blog by Leila Etaati:

https://radacad.com/interactive-map-using-r-and-power-bi-create-custom-visual-part-1

Reference in the blog this was made from, detailing pbiviz PowerBI Visual Tools:

https://www.npmjs.com/package/powerbi-visuals-tools

Required install is from here:

https://nodejs.org/

Information on customising visuals can be found in these follow-up blogs by Leila Etaati

https://radacad.com/interactive-charts-using-r-and-power-bi-create-custom-visual-part-2

https://radacad.com/create-custom-visual-with-r-and-json-part3

https://radacad.com/create-custom-visual-with-r-and-json-part-4




