# ExtractERA5HourlyData
This is a wrapper script for the [ecmwfr R package](https://github.com/bluegreen-labs/ecmwfr). It allows to extract hourly data at point locations from different types of ERA5 datasets produced by the ECMWF. For a comparison of ERA5 datasets, see [this website](https://confluence.ecmwf.int/display/CKB/The+family+of+ERA5+datasets).

## How to use
### 1. Test web-based download function
- Create an account for the Copernicus climate data storage (CDS) and get your user ID (UID) and API key, following instructions on [this website](
  [https://github.com/bluegreen-labs/ecmwfr#use-copernicus-climate-data-store-cds](https://cds.climate.copernicus.eu/)).
- Download a small dataset from [this website](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels?tab=form), thereby accepting the CDS terms and conditions.

### 2. Test R script with demo input CSV
Test the R script from this repository without changing the input CSV (PointCoords.csv).
- Install a command line tool that converts ECMWF grib data format to netcdf. For Ubuntu linux, this is the command grib_to_netcdf that can be installed via sudo apt-get install libeccodes-tools.
- Download all files from this repository (e.g. via Code -> Download ZIP above).
- Extact the ZIP file. The directory where "ExtractERA5HourlyData.R" is stored is called "working directory" in the following.
- Make sure the file "PointCoords.csv" is stored in a subfolder "Input" of the working directory.
- Install all libraries listed in the beginning of "ExtractERA5HourlyData.R".
- Adjust the variable "WorkDir" in the beginning of "ExtractERA5HourlyData.R" to match the working directory.
- Adjus the variable "FullPathTo_grib_to_netcdf" in the beginning of "ExtractERA5HourlyData.R" to match the location of the grib_to_netcdf executable (e.g. "/usr/bin/grib_to_netcdf").
- Enter your CDS credentials (from step 1) in the beginning of "ExtractERA5HourlyData.R".
- If you are behind a proxy server, adjust proxy settings in the beginning of "ExtractERA5HourlyData.R".
- Execute the script "ExtractERA5HourlyData.R".
- If everything worked, downloaded data is written to .../WorkDir/Output/ERA5_extracted_....csv

### 3. Adjust input CSV
If step 2 works as expected, adjust the file .../WorkDir/Input/PointCoords.csv to your needs.

#### 3.1 Changing requested parameters
- Different parameters are available for different datasets. A list of available parameters can be found on the overview website for the correspong dataset. For example [here](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels?tab=overview) for the dataset "ERA5 hourly data on single levels from 1979 to present".
- Note that the names of the requested parameters (column "Parameter" in PointCoords.csv) must match the parameters names for API calls. To get these names, go to the web download (step 1), select the desired parameters and click the "Show API request" button at the bottom left of the website. Some parameters name are:

| Parameter name on ERA5 overview page  | Parameter name for API calls/in PointCoords.csv|
| ------------- |-------------|
| 10m u-component of wind | 10m_u_component_of_wind |
| 10m v-component of wind | 10m_v_component_of_wind |
| 2m temperature | 2m_temperature |
| 2m dewpoint temperature | 2m_dewpoint_temperature |
| Snow cover | snow_cover |
| Snow depth | snow_depth |
| Total precipitation | total_precipitation |
| Total cloud cover | total_cloud_cover |
| Surface pressure | surface_pressure |
| Surface solar radiation downwards | surface_solar_radiation_downwards |
| Volumetric soil water layer 1 | volumetric_soil_water_layer_1 |

#### 3.2 Changing the dataset
 - To change the ERA5 dataset type, adjust the file .../WorkDir/Input/PointCoords.csv accordingly.
 - A list of datasets can be found [here](https://cds.climate.copernicus.eu/cdsapp#!/search?type=dataset).
 - Note that the name of the dataset must match the dataset name in the API call. To identify the dataset name in the API call, proceed as described above for parameter names.
 - Currently, the following datasets have been tested:

| Full name of dataset  | Dataset name for API calls/in PointCoords.csv|
| ------------- |-------------|
| [ERA5 hourly data on single levels from 1979 to present](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels?tab=overview) | reanalysis-era5-single-levels |
| [ERA5-Land hourly data from 1950 to present](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-land?tab=overview) | reanalysis-era5-land |


## Remarks
- Interpretation of precipitation amounts is not straightforward (see https://confluence.ecmwf.int/pages/viewpage.action?pageId=197702790)
-  Global radiation (hourly values): Value for Hour=0 is sum of GR of previous day (J/m2). Value for other hours is cumulative GR of the respective day (J/m2).
- Note that the ecmwfr package uses the local operating system's keyring to store the CDS login credentials, therefore you might be prompted to enter your operating system login password (see https://github.com/bluegreen-labs/ecmwfr).
- Note that the script will always downloads full months, no matter which exact days are entered in columns DateStart and DateEnd.
- Relative humidty is not available as a download parameter, but can be calculated from parameters 2m_temperature and 2m_dewpoint_temperature, for example with [this script](https://github.com/AndSchmitz/CalculateRelHumid).
- Download speed depends on server load and can be extremely slow.
- Respect the ERA5 license and citation agreements.


## Validation

- tbd
