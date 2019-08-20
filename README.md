# Income, Race, Bikes


Question:
Who decides where the bike share stations are located?  Are these decisions equitable?


### Running Visualization Locally

- `npm install`
- `npm start`
- Go to your locally running server at http://127.0.0.1:8080




## Data


### Census Data


##### About the Data

Data is from the 

Race and Income data is 


##### Obtaining and processing the Census Data


The processing is divided into two parts:
1. Processing/Creating a shapefile to visualize the census tracts
2. Obtaining and processing income/race data for each state's census tracts

The shapefile is then joined with the income/race census data.
The 2010 census tracts are used (redrawn every 10 years)

This is done for each area shown on the map.

For shapefile:
- Get shapefile for census tracts
- Possibly prune the shapefile to only include census tracts
- Process the shapefile to have geoids at census tract granularity
	- To better understand geoids, see https://www.census.gov/programs-surveys/geography/guidance/geo-identifiers.html
	- For processing, see /scripts
This shapefile is (inner-) joined with the given state's census data


For state census data:
- Download the data by going to https://factfinder.census.gov/faces/nav/jsf/pages/download_center.xhtml (OMG this site has some *wild* 90's era graphics)
	- Choose "I know the dataset or table(s) that I want to download."
	- Select "American Community Survey"
	- And then for __each year__ of interest download data for 5-year estimates (e.g. "2014 ACS 5-year estimates")
		- years of interest: beginning of bike sharing program to most recent
	- Select a geographic type: "Census Tract - 140"
	- Select state --> Add to selection --> Next
	- Select tables (can use search) and download CSVs for tables 1 by 1 (tedious)
		- race as "RACE"
		- median income as "MEDIAN INCOME IN THE PAST 12 MONTHS (IN <year> INFLATION-ADJUSTED DOLLARS)"
		- <img src="./docs/using-factfinder-ma-income.png">
	- Download each table and open zipfile.  The name of the relevant csv within looks something like "ACS_[yr]_5YR_S1903_with_ann.csv" but double check the file
	- Rename the relevant CSV as "[state]\_[yr]\_[income|race].csv" and save it in the /data/[state]/ directory
		- e.g. 2011 income data for MA from ACS 5-year estimates is saved to "/data/ma/ma_2011_income.csv"

- Process all of the csv data files for the state into one file by running /scripts/census_data_processing (with jupyter)






### Bike Station Data


Bike trips data is scraped (see scripts).
- NYC: https://s3.amazonaws.com/tripdata/index.html
- Boston: https://s3.amazonaws.com/hubway-data/index.html


This data is used to find the first trip originating from each bike dock station, which is assumed to be when that station was added to the network.

- Run the scraping scripts (/scripts/boston_bikes_data_scraper etc) to download the data locally.
- The data is then processed by the bikes data processing script.
- The output is saved to both csv and json.  That json is then copied to /viz to be used in map.

