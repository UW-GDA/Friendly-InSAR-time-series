# Friendly-InSAR-time-series
George Brencher

## Summary
This repo contains a user-friendly workflow for producing Sentinel 1 InSAR time series with ISCE 2 and MintPy.

## Background
Sentinel 1 interferometric synthetic aperture radar (InSAR) time series can reveal millimeter-scale displacement in the Earth's surface over short time scales. These time series can be used to study processes associated with deformation of volcanos, glaciers, landslides, earthquakes, groundwater, permafrost and much more. Sentinel-1 is a constellation of two satellites, Sentinel-1A and Sentinel-1B (launched in 2014 and 2016 respectively), that collect C-band (5.54 cm wavelength) synthetic aperture radar with a minimum revisit time of 6 days. C-band radar can be used to image the earth's surface independently of sunlight and cloud cover. The basic principal of InSAR is that offsets in phase between two radar images acquired at different times can be related to deformation of the ground surface along the line of sight (LOS) of the sensor (Bürgmann et al., 2000). In practice, creating a time series of LOS surface displacement from SAR data requires extensive processing. This repo contains a streamlined processing workflow for creating and visualizing Sentinel 1 InSAR time series using new and existing tools. 

## Objective
This repo contains a user-friendly, reproducible workflow for InSAR time-series processing. 

## Data
- Sentinel 1 Level 1 Single Look Complexes (SLCs) https://sentinels.copernicus.eu/web/sentinel/user-guides/sentinel-1-sar/product-types-processing-levels/level-1
- Sentinel 1 POD Precise Orbit Ephemerides data https://qc.sentinel1.eo.esa.int/aux_poeorb/
- Shuttle Radar Topography Mission (SRTM) digital elevation models (DEMs) https://www2.jpl.nasa.gov/srtm/
- MintPy example interferogram stack https://zenodo.org/record/3635245#.Yi4Ps3rMJPY

## Tools
- Jet Propulsion Laboratory (JPL) InSAR Scientific Computing Environment (ISCE) 2 (Rosen et al., 2021) https://github.com/isce-framework/isce2
- Miami INsar Time-series software in PYthon (MintPy) (Yunjun et al., 2019) https://github.com/insarlab/MintPy

## Approach
My approach consists of: 
1. Creating and unwrapping interferograms using the InSAR scientific computing environment software, making it easy to swap out digital elevation models used for topographic correction/geocoding and load SRTM if no DEM is provided
2. Nicely visualizing products including unwrapped interferograms, coherence maps, and partial and merged geometry files
3. Performing time-series inversion with The Miami INsar Timeseries software in Python
4. Nicely visualizing time series
5. Estimating uncertainty based on deformation of stable reference areas
6. Exporting data products to Google Drive 

## Outcomes
I have produced a test time series for Fernandina volcano in Galápagos.

## Workflow
### Notebook 01_download_stack-processing

This notebook creates an environment for ISCE, downloads the appropriate data, and runs ISCE topsStack. 

1. Create an environment for ISCE 2.5 and sets appropriate environment variables such that ISCE can be run from command line. 
2. Creates the appropriate directory structure for ISCE if it does not exist.
3. Use the ASF Vertex API to download scenes based on bounding box, start and end dates, season, and flight direction. 
4. Download corresponging precise orbital data from scihib.copernicus.eu



## References
Bürgmann, R., Rosen, P. A., & Fielding, E. J. (2000). Synthetic aperture radar interferometry to measure Earth’s surface topography and its deformation. Annual review of earth and planetary sciences, 28(1), 169-209.

Rosen, P. A., Gurrola, E., Sacco, G. F., & Zebker, H. (2012). The InSAR scientific computing environment. In: 9th European Conference on Synthetic Aperture Radar, 23–26 April, Nuremberg, Germany, 730–733 

Yunjun, Z., Fattahi, H., & Amelung, F. (2019). Small baseline InSAR time series analysis: Unwrapping error correction and noise reduction. Computers & Geosciences, 133, 104331.
