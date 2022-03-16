# Friendly-InSAR-time-series
George Brencher

## Summary
This repo contains a user-friendly workflow for producing Sentinel 1 InSAR time series with ISCE 2 and MintPy.

## Background
Sentinel 1 interferometric synthetic aperture radar (InSAR) time series can reveal millimeter-scale displacement in the Earth's surface with ~14 day temporal resolution. These time series can be used to study important processes associated with deformation of volcanos, glaciers, landslides, earthquakes, groundwater, permafrost and much more. Sentinel-1 is a constellation of two satellites, Sentinel-1A and Sentinel-1B (launched in 2014 and 2016 respectively), that collect C-band (5.54 cm wavelength) synthetic aperture radar with a minimum revisit time of 6 days. C-band radar can be used to image the earth's surface independently of sunlight and cloud cover. The basic principal of InSAR is that offsets in phase between two radar images acquired at different times can be related to deformation of the ground surface along the line of sight (LOS) of the sensor (Bürgmann et al., 2000). In practice, creating a time series of LOS surface displacement from SAR data requires extensive processing. This repo contains a streamlined processing workflow for creating and visualizing Sentinel 1 InSAR time series using two existing tools: topsStack and MintPy. 

## Objective
This repo was built to contain a user-friendly, reproducible workflow for InSAR time-series processing. 

## Data
- Sentinel 1 Level 1 Single Look Complexes (SLCs) https://sentinels.copernicus.eu/web/sentinel/user-guides/sentinel-1-sar/product-types-processing-levels/level-1
- Sentinel 1 POD Precise Orbit Ephemerides data https://qc.sentinel1.eo.esa.int/aux_poeorb/
- Shuttle Radar Topography Mission (SRTM) digital elevation models (DEMs) https://www2.jpl.nasa.gov/srtm/
- MintPy example interferogram stack https://zenodo.org/record/3635245#.Yi4Ps3rMJPY
- Copernicus ERA5 climate reanalysis data https://cds.climate.copernicus.eu/#!/home

## Tools
- Jet Propulsion Laboratory (JPL) InSAR Scientific Computing Environment (ISCE) 2 (Rosen et al., 2021) https://github.com/isce-framework/isce2
- Miami INsar Time-series software in PYthon (MintPy) (Yunjun et al., 2019) https://github.com/insarlab/MintPy
- xarray, https://docs.xarray.dev/en/stable/, https://github.com/pydata/xarray

## Approach
My approach consists of: 
1. Creating and unwrapping interferograms using the InSAR scientific computing environment software, making it easy to swap out digital elevation models used for topographic correction/geocoding and load SRTM if no DEM is provided
2. Visualizing products including unwrapped interferograms, coherence maps, and partial and merged geometry files
3. Performing time-series inversion with The Miami INsar Timeseries software in Python
4. Visualizing time series products
5. Finding cumulative deformation and mean velocity in areas of interest and stable reference areas from uploaded geojson files using xarray
6. Swapping and comparing stable reference points 

## Outcomes
I have produced a test time series for Fernandina volcano in Galápagos.

## Current issues
1. TopsStack is not merging geometry files correctly. Since the MintPy workflow depends on these geometry files, the workflow is currently hung up after notebook 1. 

2. xarray does not automatically read in coordinates and dimensions from HDF files output by MintPy. This issue was resolved by manually rebuilding coordinates from metadata.

3. MintPy is generating coherence maps with sawtooth patterns between areas of high and low coherence. The provenance and outcomes of this issue have not been considered. 

4. Sentinel I SLCs are ~ 4 gb each, and I have not found an easy way to subset them prior to download. Computers with limited disk space will not be able to compute long time series without rerunning the stack processor multiple times (which is a possibility, and topsStack will add to an existing stack rather than build a new one). 

## Future directions
Beyond resolving the issues outlined above, I would like to spend more time digging into understanding each processing step. I would like to experiment more with different processing options for different environments. I would like to compare results of using different DEMs. I also want to use correlation to mask time series results used in data analysis. In addition to defining areas of interest with uploaded geojson files, I would like users to be able to click out an areas of interest and points in the Jupyter notebook and visualize their deformation over time and velocity. I would also like to add a function to save time slices as geotiffs and export them to google drive. 

## Workflow
### Notebook 01_download_stack-processing

This notebook creates an environment for ISCE, downloads the appropriate data, and runs ISCE topsStack. 

1. Create an environment for ISCE 2.5 and sets appropriate environment variables such that ISCE can be run from command line. 
2. Creates the appropriate directory structure for ISCE if it does not exist.
3. Use the ASF Vertex api to download scenes based on bounding box, start and end dates, season, and flight direction. 
4. Download corresponding precise orbital data
5. Download corresponding SRTM DEM data if the user doesn't want to use an alternative
6. Set stack processing options including number of connections, coherence threshold, and number of looks in azimuth and range. Generate run files for ISCE with stackSentinel.py. 
7. Consecutively run the run files, building an interferogram stack. Along the way, geometry files are plotted.
8. Plot amplitude, phase, and coherence of interferogram stack. 

### Notebook 02_mintpy_time-series

This notebook creates an environment for MintPy, loads approapriate data into MintPy, uses MintPy to compute a time series based on the inteferogram stack, and implements corrections to the time series. 

1. Create an environment for MintPy
2. Download example data (not necessary if Notebook 01 runs correctly, which it currently does not). 
3. Write a configuration file for MintPy, which contains paths to necessary data, metadata, and processing options. Make sure to include a reference point that is close to the area of interest and appears to be coherent over time. In the name of the file, include something like 'SenD131' for Sentinel, descensing, track 131. 
4. Load data into MintPy and examine inputs. 
5. Modify, the time series, removing dates based on coherence, date, temporal or perpendicular baselines, etc. Plot the interferogram network. 
6. Do the 'quick overview' step, which stacks phase from all the interferograms to quickly estimate velocity. 
7. Invert the network, using a linear least squares method to solve for cumulative displacement with coherence as a pixel weight, and plot the resulting time series and temporal coherence. 
8. Implement corrections to the time series, including a) troposphere correction using ERA5 climate reanalysis data, b) removing phase ramps based on the phase of reliable pixels, and c) remove residual phase due to DEM error (Fattahi & Amelung, 2013) https://ieeexplore.ieee.org/document/6423275.
9. Calculate average velocity and geocode MintPy outputs

### Notebook 03_spatial-analysis

This notebook load the time series and velocity data into xarray, finds displacement and velocity in regions of interest and stable reference areas, and changes the location of the stable reference point. 

1. Load in the time series and velocity data and rebuild the coordinates using metadata. 
2. Load in polgons delineating regions of interest and stable reference areas, convert to geodataframes, and visualize polygon location and average velocity. 
3. Visualize displacement of aoi compared to stable reference areas. 
4. Visualize average velocity of aoi compared to stable reference areas. 
6. Change reference points by supplying new coordinates, compare the original reference point to the new reference point. 

## References
Bürgmann, R., Rosen, P. A., & Fielding, E. J. (2000). Synthetic aperture radar interferometry to measure Earth’s surface topography and its deformation. Annual review of earth and planetary sciences, 28(1), 169-209.

Fattahi, H., & Amelung, F. (2013). DEM error correction in InSAR time series. IEEE Transactions on Geoscience and Remote Sensing, 51(7), 4249-4259.

Rosen, P. A., Gurrola, E., Sacco, G. F., & Zebker, H. (2012). The InSAR scientific computing environment. In: 9th European Conference on Synthetic Aperture Radar, 23–26 April, Nuremberg, Germany, 730–733 

Yunjun, Z., Fattahi, H., & Amelung, F. (2019). Small baseline InSAR time series analysis: Unwrapping error correction and noise reduction. Computers & Geosciences, 133, 104331.
