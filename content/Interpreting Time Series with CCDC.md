---
title: Interpreting Time Series with CCDC
---
GEE ability: access decades of imagery without the previous limitation of needing to download all the data to a local disk for processing

Change Detection and Classification (CCDC) is a land change monitoring algorithm designed to operate on time series of satellite data.

## Theory

Adjacent observations are dependent: consecutive observations in a time series are not independent of each other, but rather each point is related to its preceding points.

### Temporal Segmentation 

#### How spectral change is detected?
Spectral change is detected at the pixel level by testing for structural breaks in a time series of reflectance. In Earth Engine, this process is referred to as “temporal segmentation,” as pixel-level time series are segmented according to periods of unique reflectance. It is done by fitting harmonic regression models to all spectral bands in the time series.

#### How the algorithm detect the changes?
The model-fitting starts at the beginning of the time series and moves forward in time in an **online approach** to change detection. The coefficients are used to predict future observations, and **if the residuals of future observations exceed a statistical threshold for numerous consecutive observations, then the algorithm flags that a change has occurred**. After the change, a new regression model is fit and the process continues until the end of the time series.

## Parameters

`minObservations`: number of consecutive observations required to flag a change

`chiSquareProbability` and `minNumOfYearsScaler` default parameters of 0.99 and 1.33 respectively, control the sensitivity of the algorithm to detect change and the iterative curve fitting process required to detect change

`lambda` and `maxIteration` parameters control the curve fitting process, higher maxIteration might take longer to complete

`tBreak`: The time segment break date if a change is detected

# Questions

What is harmonic regression models?

In EE temporal segmentation, the time series are segmented. Does the number of segments is predefined?

# Notes 

We can do a similar analysis for larger areas by first running the CCDC algorithm over a group of pixels.

The CCDC function in Earth Engine can take any `ImageCollection`. CCDC contains an internal cloud masking algorithm and is rather robust against missed clouds, but the cleaner the data the better.

We typically export the results to an Earth Engine asset first, and then inspect the asset. To avoid having issues such as taxing to the system very quickly, resulting in memory errors.

Exporting image to asset: larger tiles may take several hours to complete

The detection of a break does not always imply a change of land cover. Natural events, small-scale disturbances and seasonal cycles, among others, can result in the detection of a break by CCDC.

A time series in Earth Engine is typically represented as an ImageCollection

ImageCollection characteristics:
- At each pixel, there might be a distinct number of observations taken from a unique set of dates.
- The size (length) of the time series can vary across pixels.
- Data may be missing in any pixel at any point in the sequence (e.g., due to cloud masking).

# Reference

- Near Real Time monitoring of satellite image time-series: [github](https://github.com/ec-jrc/nrt?tab=EUPL-1.2-1-ov-file)
- Google Earth Engine Tutorials: [web](https://google-earth-engine.com/), [github](https://github.com/krishnakafle/gee-tutorials)
- CCDC GUI: [GEE App](https://parevalo_bu.users.earthengine.app/view/visualize-ccdc), [paper](# A Suite of Tools for Continuous Land Change Monitoring in Google Earth Engine)
- [gee tutorial](https://google-earth-engine.com/Interpreting-Image-Series/Interpreting-Time-Series-with-CCDC/)


