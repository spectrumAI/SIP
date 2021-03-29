# Land cover classification using Modis 250m NDVI time series

## Tutorial Objectives

* Explore the feasibility of using fully convolutional residual network for land cover classification using NDVI time series.

## Key challenges

* Strong ***signature variabilities*** of different land cover classes in NDVI time series.

## Experiments

* ***Experiment 1: Preprocess MODIS 250m products and obtain Canada NDVI time series of year 2010***

    1. Download and preprocess MODIS 250m products according to [the preprocessing tutorial](./modis_250m_preprocessing.md).
 
* ***Experiment 2: Train dip-res model using the time series blocks***

    1. Open ***config_os.yaml*** and ensure that ***raw_img_dir*** points to the folder with preprocessed time series blocks, ***raw_band_names*** consists of ***'*all-channels.tiff'***, ***dirs -> data/mask/save*** are set using your own directories, ***num_classes*** equals 19, ***is_colors_defined_in_rgb_values*** is True, and ***my_colors*** consists of 20 colors of rgb values, and ***my_classes*** consists of 20 class names. 
    1. In ***config_os.yaml***, under ***train_params***, ***lr*** equals 0.0001, ***batch_size_train/val/test/predict*** equals 2, ***net_type*** is dip-res.  

