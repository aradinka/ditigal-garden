---
title: Continuous Change Detection and Classification
---

It's an algorithm used in remote sensing area for land cover analysis. The original methods published form paper titled "Continuous change detection and classification of land cover using all available Landsat data", published in 11 February 2014. Now it's implementation are available on [[Google Earth Engine]].


---

# Abstract

- Detecting many kinds of [[Land Cover Change]] coninuously as new images are collected, and providing land cover maps for any given time
- Eliminating "noisy" observation by using masking algorithm. A two-step cloud, cloud shadow, and snow masking algorithm is used
- Time series model. It has components of seasonality, trend, and break. Estimates surface reflectance and brightness temperature. The model is updated dynamically with newly acquired observations
- CCDC algorithm use threshold derived from 7 [[Landsat]] bands due to differences in spectral response for various kinds of [[Land Cover Change]]
- Land surface change is detected when the difference between observed and predicted images exceeds a threshold three consecutive times, than a pixel is identified as land surface change
- Land cover classification is done after change detection. Coefficient from the time series model and the [[Root Mean Squared Error]] from model estimation are used as input to the [[Random Forest Classifier]]
- The CCDC algorithm is applied to one [[Landsat]] scene in New England (WRS Path 12 and Row 31). All available (a total of 519) [[Landsat]] images acquired between 1982 and 2011 were used
- CCDC results were accurate for detecting land surface change, with producer's accuracy of 98% and user's accuracies of 86% in spatial domain and temporal accuracy of 80%. The land cover map with 16 categories had an overall accuracy of 90%

Keywords: CCDC, Classification, Change detection, Time series, Land cover, Continuous, [[Landsat]], Random Forest


# Introduction

- [[Impact of Land Cover on Environmental Processes]]
- [[Natural and Anthropogenic Land Cover Change]]

Previous research:
- Clouds could reduce the amount of usable data. It's hard to find an ideal pair of [[Landsat]] images that are free of clouds, cloud shadows and snow.
- Several change detection algorithm over shorter time intervals

# Questions

- Two-step cloud and cloud shadow masking algorithm
- Difference in Landsat bands spectral response
- Temporal accuracy, producer's and user's accuracy


