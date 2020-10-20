# Entire Arctic ocean sea ice mapping from Sentinel-1 images in the melting season 

## Tutorial Objectives

* ***Explore the feasibility of using deep learning for fast and accurate large-scale sea ice mapping***

* ***Identify the best deep learning model and data processing pipeline for big complex remote sensing data processing*** (3).

## Deep learning models

* Approach (1): Pixel/patch-based CNN algorithm

## Key challenges

* Big signature variabilities of open water and sea ice due to large spatial extent, banding effect in HV channel, incidence angle effect in HH channel

* Potential inconsistency of predicted label maps among adjacent images 


## Experiments

1. ***Experiment 1: Generate Sep. 1 2019 Arctic sea ice map***

    1. Download Sentinel-1 images covering the entire Arctic ocean on Sep. 1, 2019;
    1. For each scene, train approach (1) using pixels sampels collected from the training images;
    1. Use the trained (1) to generate full label maps for the scene used for training;
    1. Calculate training and validation accuracies for this each scene;
    1. Generate entire Arctic ocean sea ice map on Sep. 1 by mosaicing all label maps of all scenes together;
 
1. ***Experiment 2: Train approach (1) using all scenes on Sep. 1 2019***

    1. Train approach (1) using pixel samples of all scenes on Sep. 1;
    1. Calculate training and validation accuracies for approach (1)
    1. ***Used the trained model to predict all training scenes. And, take a look at the label maps of these scenes. If you find some areas are misclassified, you need to add these misclassified areas as training samples and retrain the model. Repeat this process until the label maps look great!***

1. ***Experiment 3: Test trained approach 1 on all scenes on Sep. 2 2019***

    1. Download Sentinel-1 images covering the entire Arctic ocean on Sep. 2, 2019;
    1. For each scene, draw pixel-based test samples;
    1. Use the trained approach (1) to predict the label maps of all scenes on Sep. 2, 2019;
    1. Calculate test accuracies of approach (1) on Sep. 2, 2019; 
    1. Generate entire Arctic ocean sea ice map on Sep. 2 by mosaicing all label maps of all scenes together;
    1. Compare the Arctic ocean sea ice maps of Sep. 1 and Sep. 2, 2019;

1. ***Experiment 4: Test trained approach 1 on all scenes on Sep. 3 2019***

    1. Download Sentinel-1 images covering the entire Arctic ocean on Sep. 3, 2019;
    1. For each scene, draw pixel-based test samples;
    1. Use the trained approach (1) to predict the label maps of all scenes on Sep. 3, 2019;
    1. Calculate test accuracies of approach (1) on Sep. 3, 2019; 
    1. Generate entire Arctic ocean sea ice map on Sep. 3 by mosaicing all label maps of all scenes together;
    1. Compare the Arctic ocean sea ice maps of Sep. 1 Sep. 2 and Sep. 3, 2019;

1. ***Experiment 5: Generate Arctic sea ice maps for Sep. 4, 5, ..., 31, 2019***

    1. Download Sentinel-1 images covering the entire Arctic ocean for each day;
    1. For each scene, draw pixel-based test samples;
    1. Use the trained approach (1) to predict the label maps of these scenes;
    1. Calculate test accuracies of approach (1) for these days; 
    1. Generate entire Arctic ocean sea ice map on these days by mosaicing all label maps of all scenes together;
    1. Conduct time series analysis in Sep. 2019;

## Procedures of Experiment 1

* **Download data.** 
    1. Go to https://search.asf.alaska.edu/#/. Click on ***Arctic map view*** under the ***Map projection*** panel. Please copy *POLYGON((162.1385 70.5747,170.3789 69.1826,173.0147 69.9109,-171.9991 66.2364,-165.3146 66.5971,-163.8295 70.5517,-141.0027 70.2103,-131.8234 69.4745,-118.7329 69.2591,-125.7601 72.4353,-123.7074 76.5085,-109.8374 80.0968,-90.9058 82.533,-66.0815 84.1377,-58.0219 83.4228,-18.0914 83.7729,12.2722 80.3598,31.7989 81.3237,61.4568 81.9729,88.5461 81.2813,98.6308 79.982,112.9429 76.1546,116.1535 73.5502,134.7756 74.093,130.9475 72.1052,138.6538 73.7119,133.8407 77.6527,153.1613 74.8675,151.2038 72.315,162.1385 70.5747))* and paste it to **Area of Interet** in the webpage. Click ***Filters*** and set ***starting date*** and ***end date*** to be 2019.09.01, ***File type*** to be L1 GRD MD, and ***Beam Mode*** to be EW. Click on ***Search***, and you should find 59 scenes. Download them. 
    1. Copy all files to the ***sentinel1_zip_dir*** folder defined in the ***config_os.yaml*** file. 

* **Refer to [previous tutorial](./combining_pixel_image_sea_ice_sentinel1.md) Experiment 1 steps 1 to 4, for detailed procedures** 
 
## Procedures of Experiment 2

* **Refer to [previous tutorial](./combining_pixel_image_sea_ice_sentinel1.md) Experiment 1 step 6, for detailed procedures** 

* ***Used the trained model to predict all training scenes. And, take a look at the label maps of these scenes. If you find some areas are misclassified, you need to add these misclassified areas as training samples and retrain the model. Repeat this process until the label maps look great!***


## Procedures of Experiment 3

1. **Download data.** 
    1. Follow the same procedures in Experiment 1 to download the data. But, here you need to set the start date and the end date to Sep. 2, 2019. You should be able to download 54 scenes for this experiment. 
    1. Copy all files to the ***sentinel1_zip_dir*** folder defined in the ***config_os.yaml*** file. 
    1. Preprocess the zip files by following step 3, Experiment 1 in [previous tutorial](./combining_pixel_image_sea_ice_sentinel1.md). 
    1. Draw test samples by following step 4(ii), Experiment 1 in [previous tutorial](./combining_pixel_image_sea_ice_sentinel1.md). ***The only difference is that here you draw two classes, i.e., ice_test and water_test.***

1. **Refer to [previous tutorial](./combining_pixel_image_sea_ice_sentinel1.md) Experiment 1 step 7, for detailed procedures.** 
 

## Procedures of Experiment 4

* The procedures are the same with Experiment 3. The only difference is that you need to download data on Sep. 3, 2019. You should be able to find 68 scenes for this experiment. 


## Procedures of Experiment 5

1. Take a look at the label maps of the scenes in Experiment 3 and 4, and you may find some areas have been misclassified. Please re-draw these misclassified areas as training samples, and use them together with the samples in Experiment 1 to train the model. 
    
1. Use the trained model to predict the label maps on the other dates. 

    1. Copy all scenes (each with HH and HV channels) to the ***raw_data_params -> raw_img_dir*** defined in the config_os.yaml file. 
    1. Click on ***Prepare data*** under the ***Classification*** menu to get the data ready for prediction.
    1. Click on ***Predict image*** under the ***Classification*** menu to perform prediction of all scenes. 
    
1. Produce Arctic sea ice maps for each date by mosaicing label maps of all scenes on this date into a single label map. 
