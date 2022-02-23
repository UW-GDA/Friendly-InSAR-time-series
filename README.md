# Friendly-InSAR-time-series
Quinn Brencher

## Summary
This repo contains a user-friendly workflow for producing Sentinel 1 InSAR time series with ISCE 2 and MintPy.

## Background
Sentinel 1 interferometric synthetic aperture radar (InSAR) time series can reveal millimeter-scale displacement in the Earth's surface over short time scales. These time series can be used to study processes associated with deformation of volcanos, glaciers, landslides, earthquakes, groundwater, permafrost and much more. Sentinel-1 is a constellation of two satellites, Sentinel-1A and Sentinel-1B (launched in 2014 and 2016 respectively), that collect C-band (5.54 cm wavelength) synthetic aperture radar with a minimum revisit time of 6 days. C-band radar can be used to image the earth's surface independent of sunlight and cloud cover. The basic principal of InSAR is that offsets in phase between two radar images acquired at different times can be related to deformation of the ground surface along the line of sight (LOS) of the sensor. In practice, creating a time series of LOS surface displacement from SAR data requires extensive processing. This repo contains a streamlined processing workflow for creating Sentinel 1 InSAR time series using existing tools. 

## Objective
This repo is meant to create a user-friendly, reproducible workflow for InSAR time-series processing. 

## Data
- Sentinel 1 Level 1 Single Look Complexes (SLCs) https://sentinels.copernicus.eu/web/sentinel/user-guides/sentinel-1-sar/product-types-processing-levels/level-1
- Sentinel 1 POD Precise Orbit Ephemerides data https://qc.sentinel1.eo.esa.int/aux_poeorb/
- Shuttle Radar Topography Mission (SRTM) digital elevation models (DEMs) https://www2.jpl.nasa.gov/srtm/

## Tools
- Jet Propulsion Laboratory (JPL) InSAR Scientific Computing Environment (ISCE) 2 https://github.com/isce-framework/isce2
- Miami INsar Time-series software in PYthon (MintPy) https://github.com/insarlab/MintPy

## Approach

## Expected Outcomes

## References

