# Pavement-Widths
A methodology on how to create pavement widths data using OS Select+Build

## Description
Ordnance Survey (OS) and Transport for West Midlands (TfWM) recently completed a project exploring the creation of pavement widths data. The focus of this project was to support TfWM’s ongoing work in transport planning, road space reallocation and linear referencing.  

Following on from the success of the project, this piece details the output from that work, illustrating a methodology that can be used to determine pavement widths. This methodology creates transects by using data from the OS National Geographic Database (NGD), accessed using OS’s new easy-to-use Select+Build tool. 

Two workbenches have been created. The first uses the FME 2020 release, and the second using FME 2022. This is due to a slight change in the transformers process between the versions. 

For more information on OS Select+Build and the OS data available to developers, business, and government, visit the OS Data Hub.  

**Please note: This is a methodology and not an OS product. The methodology can be taken and adapted, but OS will not be maintaining this methodology.**

### Methodology 

To simplify this the methodology has been broken down into three sections: data, published parameters, and method overview. The method uses the following software and data: 

* Feature Manipulation Engine, known as FME (it’s recommended that you have version 2020.0.1.0 or later) 

* OS NGD data 

### Data 

OS NGD data is used because of its detailed attribution and ease of filtering pavements and paths from the Transport Features theme. The FME workbench uses OS data in a GeoPackage format. 

How to access this OS data: 

  1. Create a recipe in OS Select+Build on the OS Data Hub by expanding Transport theme > Transport Features > Road Track Or Path.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; a. Apply a filter to this data to include ‘Path’, ‘Path and Steps’, ‘Pavement’, ‘Pavement and Steps’. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; b. Create the recipe. You will then need to create a data package in a GeoPackage (GPKG) format (as this is what the FME workbench uses) for the area you require.  We’ve used GeoPackage because of its plug-and-play nature, but other formats can be used if the workbench is adapted. 

### Published Parameters  

The published parameters are variables created to make using the workbench easier and allows values to be easily changed depending on the project. Below is an explanation of each of the parameters, along with their default values. All parameters can be changed by using the dialogue box that appears when you begin running the workbench.  

a. AOI name: for ease of identification, the user is prompted to give the Area of Interest a name. This is used in the file name of the dataset that is produced. 

b. AOI file location: a directory prompt for the location of the Area of interest (GPKG). This can be any size but smaller areas will run significantly faster. 

c. NGD Paths and Pavements file location: a directory prompt for the location of the Select+Build NGD pavement and path data downloaded from OS datahub. This needs to cover the whole of the area you are looking to analyse. 

d. Transect Spacing: the distance (metres) between transects along pavements and paths. By default, this is set to 1m as it provides a good level of granularity but can be changed if required. A lower number will result in more transects. 

e. Line Extension in metres: this is how long the transect lines are extended across pavements and paths before they are clipped to the underlying polygon dataset. The default has been set as 10m because it provides coverage in most situations, whilst keeping the process fast. It can be set between 5m and 30m if required which is useful when dealing with particularly wide and large pedestrian areas. 

f. Allowable Overlapping Lines: due to complex geometries and how the underlying data is collected there are instances where transects pass over others, such as tight corners, dead end streets and cul-de-sacs. The analysis output can look complicated and cluttered, but by setting the maximum allowable overlaps to a low number, transects across features such as road bends can be retained whilst transects that are the result of odd shapes are removed. By default, this is set to 4 as it provides a good compromise, but this can be changed in this parameter if required. 

g. Output Centre Lines: this is a prompt which allows you to choose whether to extract the centre line as a line geometry. 

h. Save Location: a directory prompt for file save location. The file will be saved here named 'AOI Name'_Pavements_'todays date' as a GPKG. The format can be changed in the feature writer. 

i. Advanced Analysis (optional): an optional addition to the workbench that allows you to remove any transects that do not intersect either edge of the path/pavement. Select Yes if you would like this. 


### Method Overview 

This next section provides a brief overview of the methodology within the workbench. 

  2. The pavement and path GPKG from OS Select+Build and the AOI are read into the workbench. 

  3. Generating the centreline & transects:   

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; a. A centreline is generated from the Path and Pavement polygon from OS Select+Build in order to create the transects. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; b. Transects are created at a user specified distance (see transect spacing parameter) along the centreline and rotated 90 degrees across the Path and Pavement features. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; c. Transects are extended to a user specified distance (see line extension in metres parameter) either side of the centreline so the full cross-section of the polygons are captured.   

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; d. Transects are then clipped to the pavement/path data and any remnants are removed. 

  4. Transect Testing 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; a. Transects are tested by checking to see if a transect intersects its own centreline. If it does, then it passes.   

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; b. If a transect crosses too many other transects, it is removed (as set in the allowable overlapping lines parameter). 

  5. Data Output 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; a. The data is appended with the transect width figure in metres based on the Transect ID. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; b. A single GeoPackage is created with a name based on the AOI Name parameter and date.  

## Licence
The contents of this repository are licensed under the [Open Government Licence 3.0.](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/)

