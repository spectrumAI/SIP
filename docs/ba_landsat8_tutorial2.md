# Burned area detection from Landsat8 images using the SIP software

## Background

Efficient and accurate mapping of burned areas is critical to monitor and evaluate the influence of wild fires that greatly influence the vegetation structure, biodiversity and carbon balance. Comparing with coarse resolution burned area products, the use of medium resolution Landsat8 images allows higher-resolution burned area maps that is more suitable for regional scale studies. Deep convolutional neural network (DCNN), due to its excellent capability to learn discriminative spatial-spectral features and its GPU computational efficiency, offers an efficient way for fast and accurate burned area detection from Landsat8 images. Nevertheless, several key issues needed to be addressed for achieving efficient DCNN models in support of Landsat8 burned area detection. 

<!--
## Objectives

1. Evaluate the contribution of Landsat8 bands to the DCNN model and select the "best" bands subset to feed the DCNN model. 
1. Compare  DCNN model with traditional machine learning models, e.g., supported vector machine (SVM) and random forest (RF).
1. Evaluate the importance of the spatial component and the spectral component in DCNN model. 
1. Test the performance of the trained DCNN model on data of different years, and evaluate its temporal generalization capability.      
-->

## Tutorials

* [Burned area mapping using DCNN](./ba_landsat8_bands_selection.md)
<!--* [Compare DCNN with traditional classifiers](./ba_landsat8_compare_with_classifiers.md)
* [Evaluate spatial and spectral components in DCNN](./ba_landsat8_spatial_spectral_DCNN.md)
* [Test on data of different years](./ba_landsat8_test_on_different_years.md) -->


## References

1. Hawbaker, T. J., Vanderhoof, M. K., Schmidt, G. L., Beal, Y. J., Picotte, J. J., Takacs, J. D., ... & Dwyer, J. L. (2020). The Landsat Burned Area algorithm and products for the conterminous United States. Remote Sensing of Environment, 244, 111801.

2. Koutsias, N., & Karteris, M. (2000). Burned area mapping using logistic regression modeling of a single post-fire Landsat-5 Thematic Mapper image. International Journal of Remote Sensing, 21(4), 673-687.

3. Syifa, M., Panahi, M., & Lee, C. W. (2020). Mapping of post-wildfire burned area using a hybrid algorithm and satellite data: the case of the camp fire wildfire in California, USA. Remote Sensing, 12(4), 623.


