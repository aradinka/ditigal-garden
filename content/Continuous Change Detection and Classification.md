---
title: Continuous Change Detection and Classification
---

It's an algorithm used in remote sensing area for land cover analysis. The original methods published form paper titled "Continuous change detection and classification of land cover using all available Landsat data", published in 11 February 2014. Now it's implementation are available on [[Google Earth Engine]].


---

# Abstract

- Detecting many kinds of [[Land Cover Change]] coninuously as new images are collected, and providing land cover maps for any given time.
- Eliminating "noisy" observation by using masking algorithm. A two-step cloud, cloud shadow, and snow masking algorithm is used.
- Time series model. It has components of seasonality, trend, and break. Estimates surface reflectance and brightness temperature. The model is updated dynamically with newly acquired observations.
- CCDC algorithm use threshold derived from 7 [[Landsat]] bands due to differences in spectral response for various kinds of [[Land Cover Change]].
- Land surface change is detected when the difference between observed and predicted images exceeds a threshold three consecutive times, than a pixel is identified as land surface change.
- Land cover classification is done after change detection. Coefficient from the time series model and the [[Root Mean Squared Error]] from model estimation are used as input to the [[Random Forest Classifier]].
- The CCDC algorithm is applied to one [[Landsat]] scene in New England (WRS Path 12 and Row 31). All available (a total of 519) [[Landsat]] images acquired between 1982 and 2011 were used
- CCDC results were accurate for detecting land surface change, with producer's accuracy of 98% and user's accuracies of 86% in spatial domain and temporal accuracy of 80%. The land cover map with 16 categories had an overall accuracy of 90%.

Keywords: CCDC, Classification, Change detection, Time series, Land cover, Continuous, [[Landsat]], Random Forest.

---
# Introduction

### Monitoring land cover change with remote sensing

Reading:

- [[Impact of Land Cover on Environmental Processes]]
- [[Natural and Anthropogenic Land Cover Change]]
- [[Challenges on land cover change detection]]

### Land cover classification

Multitemporal images improve classification accuracy, especially for vegetation because of the unique phenological characteristics of different vegetation types. The problem is they need all dates of images to be free of clouds which often not possible.

We typically assume no land cover change in the time interval between the different images. This assumption is not always valid, especially when images from long time intervals are used.

Remote sensing community needs a classification algorithm that:

1. Increases the time period over which land cover maps remain "current".
2. Works for areas where multiple kinds of land cover change is common.
3. Makes land cover maps comparable between times for identification of change.

### Data

Study area is located in coastal New England, United States. It includes a wide variety of environments and land covers including; extensive urbanization, abandonment of agricultural fields, and forest clearing. And it's rare to find a cloud-free [[Landsat]] image in this area, making it an outstanding place to test the robustness of this new algorithm.

---

# Methods

## Image Preprocessing

##### Atmosphere Correction

Atmospheric correction is performed using the Landsat Ecosystem Disturbance Adaptive Processing System (LEDAPS) atmospheric correction tool.

##### Screening of cloud, cloud shadow, and snow

Initially masked using object-based algorithm called [[Fmask]]. 

##### Second step cloud preprocessing

Further screening outliers missed by the [[Fmask]] algorithm based on multitemporal analysis. This approach first estimate the time series model on "clear" observations and then detect outliers by comparing model estimates and [[Landsat]] observations. 

The [[Robust Iteratively Reweighted Least Squares]] method is used for estimating the time series model. This method reduces the influence of ephemeral changes, or pixel affected by clouds, shadows, or snow that were not identified by [[Fmask]]. 

The time series model estimates for Band 2 and 5 because clouds and snow make Band 2 brighter, also cloud shadows and snow make Band 5 darker. By comparing actual [[Landsat]] observations and the corresponding model predictions, it is comparatively easy to identify any remaining clouds, cloud shadows, snow, and other ephemeral changes.

> The observations that identified as an outlier is removed from further analysis.

The model estimation starts when there are a total of 15 "clear" (determined by [[Fmask]]) observations. The first 12 are compared between observed and model predicted values to decide whether there are outliers.

**Examples**

> [!info] Snow and cloud make Band 2 more brighter (Figure A & B)
> ![[Screenshot 2023-12-30 at 23.35.58.png]]

> [!info] Cloud shadows and snow make Band 2 more brighter (Figure C & D)
> ![[Screenshot 2023-12-30 at 23.40.26.png]]

Those labeled cloud, cloud shadows and snow are determined by the multitemporal analysis. CCDC algorithm only applies multitemporal detection during the time of model initialization. For observations acquired after model initialization, only [[Fmask]] algorithm is used.

### CCDC Time Series Model

Land surface change devided into 3 categories; Intra-annual change, Gradual inter-annual change, 

> [!info] Intra-annual change (Seasonality)
> Caused by vegetation phenology driven by seasonal patterns of environmental factors like temperature and precipitation
> ![[Screenshot 2023-12-30 at 23.47.13.png]]

> [!info] Gradual-annual change (Trend)
> Caused by climate variability, vegetation growth or gradual change in land management or land degradation
> ![[Screenshot 2023-12-30 at 23.47.21.png]]

> [!info] Abrupt change (Break)
> Caused by deforestation, floods, fire, insects or urbanization.
> ![[Screenshot 2023-12-30 at 23.47.27.png]]

Therefore, the time series model has component's of seasonality, trend, and breaks that captures all 3 categories surface change. The model coefficients are estimated using [[Ordinary Least Squares]] based on the remaining clear [[Landsat]] observations. [[Ordinary Least Squares]] is used instead of [[Robust Iteratively Reweighted Least Squares]] because it's faster and more accurate when all the significance outliers have been excluded.

### Continuous Change Detection

- To detect forest change, a single change index with a fixed threshold is sufficient.
- [[Root Mean Squared Error]] is computed for each spectral band. The difference between observations and model predictions for each band is normalized by three times the [[Root Mean Squared Error]]. We use three times the [[Root Mean Squared Error]] due to the fact that the spectral signals usually deviate from model prediction by more than three times.

> [!info] Detect change
> Fig. 6A shows the model prediction and three times the RMSE before change occurred. 
> Fig. 6B shows the model prediction and three times the RMSE when change occurred. 
> Fig. 6C shows the model prediction and three times the RMSE after change occurred.
> ![[Screenshot 2023-12-31 at 17.09.27.png]]




---
# Questions

- Two-step cloud and cloud shadow masking algorithm
- Difference in Landsat bands spectral response
- Temporal accuracy, producer's and user's accuracy
- Temporal vs multitemporal images

# Notes

Paper limitation:

- This study is a “prototype” for continuous change detection and classification using all available Landsat data. Testing this approach for other regions with different environments will be a future research direction.

Reading:

- [[Robust Iteratively Reweighted Least Squares]] 
- [[Fmask]] 

