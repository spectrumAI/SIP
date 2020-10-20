# Sea ice mapping from Sentinel-1 SAR images using the SIP software

## Background

The information of sea ice in polar regions is crucial for various applications including 
climate change study and marine navigation. As an integral part of the Earth’s climate system, 
sea ice interacts with both the ocean and the atmosphere and modulates the heat and moisture fluxes.
In particular, the extent and distribution of sea ice relative to open sea water are important
indicators of the contraction of Arctic ice caps, and has been recognized as essential climate
variables by both world meteorological organization (WMO) and the United Nations framework
convention on climate change (UNFCCC). Moreover, sea ice and open water distribution
information is also important for ship navigation along Arctic sea routes that greatly shorten the
marine transportation distance between northwestern Europe and northeastern Asia.

Satellite synthetic aperture radar (SAR), due to its capability to penetrate the cloud and work
day and night, provides a powerful tool for sea ice monitoring. The Sentinel-1 satellite generates
immense free SAR images over the Arctic area, allowing operational monitoring of sea ice
extend and dynamics. Nevertheless, fast and accurate discrimination of sea ice and open water
from Sentinel-1 SAR imagery using traditional machine learning techniques is difficult due to the
huge data volume, sensor limitations, and the complex ocean state. To address these problems
for large-scale sea ice mapping, it is important to investigate the GPU-boosted deep neural
network approach that is very efficient at handling the big complex data.

## Overall Objective

  * We have initiaed a project that aims to use the SIP software system with innovative deep neural network models
tailored to the Sentinel-1 SAR sea ice characteristics for fast and accurate production of sea ice
maps over large Arctic area using big Sentinel-1 SAR data in support of the climate change
study and Arctic ship navigation, with the following sub-objectives:

  * Design novel convolutional neural network (CNN) models and algorithms that are capable of not only
efficiently capturing the subtle textual signature of sea ice from Sentinel-1 imagery but also
accurately preserving the edges and boundaries between semantic classes. 

  * Optimize SIP to allow the use of desktop-based computational power for processing a large number of Sentinel-1 images
in a meaningful spatial-temporal scale. Also, based on SIP, build an online cloud-based operational sea ice mapping system
that generate high-precision pixel-level sea ice maps and ice charts that benefits various climate
change researchers and end-users.

## Tutorials

* [Combining pixel-based and image-based CNNs for sea ice mapping from Sentinel-1 images](./combining_pixel_image_sea_ice_sentinel1.md)
* [Sea ice mapping of the entire Arctic ocean using Sentinel-1 images](./entire_Arctic_sea_ice_sentinel1.md)

## References

1. Boulze, Hugo, Anton Korosov, and Julien Brajard. "Classification of sea ice types in Sentinel-1 SAR data using convolutional neural networks." Remote Sensing 12.13 (2020): 2165.

2. Park, Jeong-Won, et al. "Classification of sea ice types in Sentinel-1 synthetic aperture radar images." The Cryosphere 14.8 (2020): 2629-2645.


