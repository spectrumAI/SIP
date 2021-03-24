# Mapping different ice types in Arctic ocean using Sentinel-1 images

## Tutorial Objectives

* Explore the feasibility of using ***deep learning for discriminating different ice types, i.e., old ice, first-year ice, young ice, grey ice, new ice and open water***.

## Deep learning models

* Approach (1): Pixel/patch-based CNN algorithm
* Approach (2): Fully convolutional network (FCN) algorithm

## Key challenges

* Big ***signature variabilities*** of different sea ice types due to large spatial extent, banding effect in HV channel, incidence angle effect in HH channel

## Experiments

* ***Experiment 1: Preprocess and mosaic Sentinel-1 scenes on Feb. 8, 2019***

    1. Download Sentinel-1 images ***covering the entire Arctic ocean on Feb. 8, 2019***; 
    1. Preprocess downloaded scenes to generate a big mosaic; 
 
* ***Experiment 2: Download and examine the ground truth ice chart***

    1. Download the ice chart covering the Arctic ocean on Feb. 8, 2019. 
    1. Open ice chart and the mosaic image in QGIS software to identify different ice types. 

* ***Experiment 3: Draw training and validation samples in SIP and perform classification***

    1. Open mosaic HV image in SIP, and draw training and validation samples for different ice types;
    1. Perform model training;
    1. Record the training and validation accuracies;
    1. Generate ***map for the entire Arctic ocean on Feb. 8, 2019***;

* ***Experiment 4: Test the trained model on Feb. 7, 2020***

    1. Download Sentinel-1 images covering the entire Arctic ocean on Feb. 7, 2020;
    1. Download ice chart on Feb. 7, 2020. 
    1. Preprocess and mosaic the scenes as in Experiment 1;
    1. Draw test samples according to ice chart;
    1. Use the trained model in Experiment 3 to predict the label maps of mosaic HH and HV images;
    1. Obtain ***test accuracies***, and compare with training and validation accuracies in Experiment 3;
    1. Generate ***map for the entire Arctic ocean on Feb. 7, 2020***;

* ***Experiment 5: Train Approach (2) on Feb. 8, 2019***
    1. Open the mosaic image of Feb. 8, 2019 in SIP, and draw more polygon training samples for different classes. Why do we need more training samples here? The approach (2), i.e., FCN algorithm, needs more polygon-based training samples to achieve better results. 
    1. Generate training and validation masks.
    1. Split the mosaic images and associated training and validation masks into small sub-images, e.g., 256-by-256 images. These sub-images will be used as training samples for training the FCN algorithm. 
    1. Train FCN model.
    1. Predict all sub-images that are used for training to get label maps of sub-images. 
    1. Mosaic all label maps of sub-images into a big label map that covers the whole Arctic area. 
 
* ***Experiment 6: Test the trained approach (2) model on Feb. 7, 2020***
    1. Split the mosaic image of Feb. 07, 2020 into sub-images.
    1. Use the trained FCN model in Experiment 5 to predict all sub-images of Feb. 07, 2020 to generate label maps of sub-images.
    1. Mosaic all label maps of sub-images into a big label map that covers the entire Arctic area.
    1. Compare the label maps of Feb. 08, 2019 and that of Feb. 07, 2020. Compare the accuracies of these two maps. 


## Procedures of Experiment 1

* **Download data.** 
    1. Go to https://search.asf.alaska.edu/#/. Click on ***Arctic map view*** under the ***Map projection*** panel. Please copy *POLYGON((162.1385 70.5747,170.3789 69.1826,173.0147 69.9109,-171.9991 66.2364,-165.3146 66.5971,-163.8295 70.5517,-141.0027 70.2103,-131.8234 69.4745,-118.7329 69.2591,-125.7601 72.4353,-123.7074 76.5085,-109.8374 80.0968,-90.9058 82.533,-66.0815 84.1377,-58.0219 83.4228,-18.0914 83.7729,12.2722 80.3598,31.7989 81.3237,61.4568 81.9729,88.5461 81.2813,98.6308 79.982,112.9429 76.1546,116.1535 73.5502,134.7756 74.093,130.9475 72.1052,138.6538 73.7119,133.8407 77.6527,153.1613 74.8675,151.2038 72.315,162.1385 70.5747))* and paste it to **Area of Interet** in the webpage. Click ***Filters*** and set ***starting date*** and ***end date*** to be 2019.02.08, ***File type*** to be L1 GRD MD, and ***Beam Mode*** to be EW. Click on ***Search***, and you should find 59 scenes. Download them. 

* **Preprocess and Mosaic all downloaded scenes.**
    1. Copy ***config_os.yaml.bak*** to ***config_os.yaml*** and change all directories to your own directories.
    1. Copy all downloaed zip files to the ***input_dir_unzip_s1*** folder defined in the ***config_os.yaml*** file. 
    1. Run SIP software, and click on the ***Sentinel1*** item under the ***Preprocessing*** menu. 
    1. Select the ***input_dir_unzip_s1*** folder first and then select the ***config_os.yaml*** file. 
    1. SIP will sequentially run ***(1) Preprocessing (including denoising and multilooking)***, ***(2) Geocoding and reprojecting Geotiff to Polar mapping system***, ***(3) Performing land masking using coastline shapefile*** and ***(4) Mosaicing all scenes into big images***. 

* **Take a look at mosaic results**
    1. Once it finished, go to ***output_dir_merge_s1*** folder defined in ***config_os.yaml*** to make sure that there are three mosaic images, i.e., ***HH, HV and background_mask***. 
    1. Open the mosaiced ***HH, HV and background_mask*** images in QGIS to take a look. You can also open the ***coastline.shp*** file under ***SIP/data/coastline_shp*** folder to see whether the mosaics align well with the coastline. 

## Procedures of Experiment 2

* **Download data**
    1. Go to https://www.natice.noaa.gov/products/weekly_products.html, and dwonload the ***arctic190208.zip*** shapefile;

* **Open in QGIS**
    1. Open in QGIS also ***mosaic HH, HV*** images generated in Experiment 1;
    1. Open in QGIS the ***coastline.shp*** in ***SIP/data/coastline_shp*** folder; 
    1. Unzip the arctic190208.zip and open the ***.shp*** file in QGIS;

* **Interpret the code of different polygons**
    1. Now, you see many polygons of the same color. Each polygon contains some ice classes. ***How do you know which classes are contained in a polygon?*** Click the ***Identify Features*** button, and then click on a polygon. Now, in QGIS, you should see features of this polygon, e.g., CT, CA, CB, etc. 
    1. ***What do these symbols mean?*** 
    1. ***CT=81*** means 81% of the pixels in this polygon is covered by ice. 
    1. ***CA=70, CB=20, CC=10*** means the ***thickest*** ice in this polygon has a percentage of 70%, the ***second thickest*** takes 20%, and the ***third thickest*** takes 10%. 
    1. ***How do I know ice types of these CA, CB and CC?*** 
    1. That is by looking at ***SA, SB and SC*** respectively. If ***SA=95***, then ***CA*** is ***old ice***. If ***SA=85***, then ***CA*** is ***Grey-White ice***. ***How do I interpret these code, e.g., 95, 85?*** See Table 4.2 in https://library.wmo.int/doc_num.php?explnum_id=9270.  

* **What if SA, SB and SC tell different ice types? Which one to trust?**
    1. If ***SA=old_ice*** but ***SB=first_year_ice***, then, ***does this polygon belong to old_ice or first_year_ice***? 
    1. It is by looking at values of ***CA and CB***. If ***CA>CB***, then there are more old ice than first year ice, and so it make sense to assign the whole polygon to be old ice, if you can only choose one class. 
    1. You need to determine the class label for each polygon according to this rule. 

* **Identify the ice type of different polygons**
    1. Now, all polygons should appear the same color. To display different colors for different ice types, you can use this tutorial: http://www.qgistutorials.com/fr/docs/basic_vector_styling.html. But, in the ***Column*** text field, click on the ***right most math symbol*** to open the ***Expression Dialog***. Type the following code in the left part of the dialog:
    1. ```("CA">="CB" AND "CA">="CC")*"SA" + ("CB">"CA" AND "CB">"CC")*"SB" +("CC">"CA" and "CC">"CB")*"SC"```
    1. Now, if ***CA>CB>CC***, ***SA*** will be used. Otherwise, if ***CB*** is the biggest, ***SB*** will be used, and ***SC*** will be used if ***CC*** is the biggest. 
    1. You need to flip between the ice chart layer and the HV mosaic image layer to see what different ice types looks like. 

* **Combine many ice types into 6 classes**
    1. Six different classes (***open water, new ice, young ice, grey ice, first-year ice, old ice***) will be classified in SIP. 
    1. So, you need to find and remember where these six classes are, in order to draw traning and validation samples in SIP. 
    1. In ice chart, there are ***many different first-year ice types***. Here, different first year ice types in ice chart are treated as being one class, i.e., first-year ice, and Grey-white ice and grey ice are combined into grey ice, as in Reference 1 and 2 below.  

## Procedures of Experiment 3

* **Draw training and validation samples**
    1. Based on the information you see in QGIS about the six classes, draw training and validations samples for these six classes. For each class, you need to define ***train*** and ***val***. So, you will need to draw samples for 12 groups, i.e., ***old_train, old_val, first_train, first_val, young_train, young_val, grey_train, grey_val, new_train, new_val, water_train, water_val***. 

* **Train classifier**
    1. Open ***config_os.yaml***. Make sure the number of classes to be 6, and  names of the classes are ***old, first, young, grey, new, and water***.

    1. Go to the previous tutorials to see how to draw training and validation samples, and how to perform model training.

* **Examine the results**
    1. Open the classification map in QGIS and compare it with the HV mosaic image, to find some misclassification areas. Please correct these misclassifications areas by drawing more samples on these areas and re-train the classifier. Repeat this process until you find the generated map looks good enough to be consistent with the ice chart and your visual interpretation. 

## Procedure of Experiment 4 

* **Download data.** 
    1. Follow the same procedures in Experiment 1 to download the data. But, here you need to set the start date and the end date to Feb. 7, 2020. You should be able to download 56 scenes for this experiment.
    1. Go to https://www.natice.noaa.gov/products/weekly_products.html, and download shapefile ice chart for Feb. 7, 2020.

* **Preprocess and mosaic**
 
    1. Preprocess and mosaic all scenes on Feb. 7, 2020, according to steps in Experiment 1. 

* **Draw test samples and prepare data**
    1. Locate and identify the six classes in QGIS by looking both at the ice chart and the mosaic HV image. 
    1. Draw samples in SIP for six groups, i.e., ***old_test, first_test, young_test, grey_test, new_test, water_test***. 
    1. ***Generate mask*** and ***prepare data*** under the ***Classification*** menu.

* **Perform testing**

    1. Perform test on the mosaic HH and HV images, and generate maps and test accuracies by following the procedures in previous tutorials. 

    1. Open the generated classificaiton map in QGIS and compare it with the HV image and the ice chart.
    1. Also, compare it the classification map on Feb. 8, 2019. 

## Procedure of Experiment 5

* **Draw more training samples**

    1. In SIP, open the mosaic HV image on Feb. 8, 2019 from Experiment 1. Draw more polygon-based training samples for different classes. You can also increase the number of validation samples for different classes. 

    1. In SIP, generate training and validation masks to include the new samples. 

* **Generate small blocks**

    1. Open the ***config_os.yaml*** file, make sure ***to_generate_small_blocks_s1*** is ***True***, and the ***generate_small_blocks_s1_id_str*** contains the string patterns for finding all relevant files you want to process. 

    1. By default, ***training_sample_thrd_s1: 1000*** means that any blocks that have less than 1000 samples will be removed from model training by moving these blocks into another folder with name ***removed_blocks_training*** under ***output_dir_small_blocks_s1***. 

    1. You will need to change 1000 to be a smaller value if you did not draw a lot of poloygons as training samples and most of your ground truth samples are obtained by drawing lines. 

    1. Also make sure that all the other preprocessing options, e.g., ***to_merge_s1***, are all set to ***False***, because you just want to generate small blocks. 

    1. In SIP, click on ***Preprocessing -> Sentinel1***, and then select the zip folder and then the config_os.yaml file you just edited to generate small blocks.

* **Prepare data using the small blocks**
    1. In config_os.yaml, change ***raw_data_params -> raw_img_dir*** to the ***output_dir_small_blocks_s1*** folder in the previous step. 

    1. In SIP, click on ***Prepare data*** to generate the ***data_file.yaml*** file under ***dirs -> data -> train*** folder defined in config_os.yaml. Take a look at this data_file.yaml and make sure the files names are correct.  

* **Train Approach 2 FCN model**

    1. Open the ***config_os.yaml*** file, make sure the ***net_type*** is ***dip_res***. By default,we have the following settings ***lr: 0.01***, ***epoch: 200***, ***batch_size_train: 16***, ***batch_size_val: 16***, ***batch_size_predict: 16***. 

    1. In SIP, train model using the config_os.yaml you just edited. 

* **Predict both training blocks and removed blocks**

    1. After training finished, the images used for training will be automatically predicted, and label maps will be saved in the ***dirs -> save -> train*** folder defined in ***config_os.yaml***. You can go to this folder to take a look at the labels maps generated. 

    1. If ***training_sample_thrd_s1*** in config_os.yaml is 1000, then some blocks with less than 1000 training pixels will have been removed from training, and been moved to a folder named ***removed_blocks_training*** under the ***output_dir_small_blocks_s1*** folder. You need to predict these removed blocks using the trained model. 

    1. In config_os.yaml, change ***raw_data_params -> raw_img_dir*** to the ***removed_blocks_training*** folder in the previous step. 

    1. In SIP, click on ***Prepare data*** to generate the ***data_file.yaml*** file under ***dirs -> data -> train*** folder defined in config_os.yaml. Copy this data_file.yaml and paste to ***dirs -> data -> predict*** folder and replace the existing file. 

    1. In SIP, click on ***Predict image*** and select the config_os.yaml file you edited to predict all blocks under the ***removed_blocks_training*** folder. 

    1. Once prediction is finished, all label maps of removed blocks will be put in the ***dirs -> save -> predict*** folder defined in config_os.yaml. You can go to this folder to take a look. 

    1. Now, you need to copy all label maps of training blocks in the ***dirs -> save -> train*** folder, and paste them into the ***dirs -> save -> predict*** folder to merge with the label maps of the removed blocks. 

* **mosaic all label maps of sub-images/blocks**

    1. In config_os.yaml, make sure ***to_merge_s1*** is ***True***, and ***all other preprocessing options are False***, e.g., ***to_generate_small_blocks_s1*** is ***False***. 

    1. Make sure ***input_dir_merge_s1*** points to ***dirs -> save -> predict*** folder in config_os.yaml. Make sure ***merge_s1_id_str*** only has one string pattern, i.e., ```'*geocoded.tiff'```. Remove all the other string patterns by commenting them using '#'. 

    1. In SIP, click on ***Preprocessing -> Sentinel1***, then select the zip folder and config_os.yaml to merge all label maps of the sub-images/blocks.

    1. Once finished, you can go to ***output_dir_merge_s1*** folder defined in config_os.yaml to take a look at the mosaic label map covers that whole Arctic area. Open it in QGIS to examine the accuracy. 

## Procedures of Experiment 6

* **Prepare config file**
    1. Copy config_os.yaml in Experiment 5 to the 2020.02.07_arctic folder.
    
    1. Open config_os.yaml, change all folders from 2019.02.08_arctic to be 2020.02.07_arctic.

* **Split mosaic images to generate small blocks for Feb. 07, 2020***
    1. The procedures are the same as in Experiment 5. 

    1. One difference is that you need to set ***to_remove_blocks_with_limited_training_pixels_s1*** to be ***False***, such that no ocean blocks will be removed. 
    
    1. Another difference is that, when you run ***Preprocessing -> Sentinel1*** in SIP, you need to select the 2020.02.07_arctic folder, not the 2019.02.08_arctic folder. 

    1. Once finished, all blocks should be in the ***output_dir_small_blocks_s1*** folder defined in config_os.yaml. 

* **Predict all blocks***

    1. In config_os.yaml, change ***raw_data_params -> raw_img_dir*** to the ***output_dir_small_blocks_s1*** folder in the previous step. 

    1. In SIP, click on ***Prepare data*** to generate the ***data_file.yaml*** file under ***dirs -> data -> train*** folder defined in config_os.yaml. Copy this data_file.yaml and paste to ***dirs -> data -> predict*** folder and replace the existing file. 

    1. In SIP, click on ***Predict image*** and select the config_os.yaml file you edited to predict all blocks under the ***removed_blocks_training*** folder. 

    1. Once prediction is finished, all label maps of removed blocks will be put in the ***dirs -> save -> predict*** folder defined in config_os.yaml. You can go to this folder to take a look. 

* **mosaic all label maps of sub-images/blocks**

    1. In config_os.yaml, make sure ***to_merge_s1*** is ***True***, and ***all other preprocessing options are False***, e.g., ***to_generate_small_blocks_s1*** is ***False***. 

    1. Make sure ***input_dir_merge_s1*** points to ***dirs -> save -> predict*** folder in config_os.yaml. Make sure ***merge_s1_id_str*** only has one string pattern, i.e., ```'*geocoded.tiff'```. Remove all the other string patterns by commenting them using '#'. 

    1. In SIP, click on ***Preprocessing -> Sentinel1***, then select the zip folder and config_os.yaml to merge all label maps of the sub-images/blocks.

    1. Once finished, you can go to ***output_dir_merge_s1*** folder defined in config_os.yaml to take a look at the mosaic label map covers that whole Arctic area. Open it in QGIS to examine the accuracy. 

    1. Compare this label map with the label map of 2019.02.08 in QGIS. 

## References

1. Boulze, Hugo, Anton Korosov, and Julien Brajard. "Classification of sea ice types in Sentinel-1 SAR data using convolutional neural networks." Remote Sensing 12.13 (2020): 2165.

2. Park, Jeong-Won, et al. "Classification of sea ice types in Sentinel-1 synthetic aperture radar images." The Cryosphere 14.8 (2020): 2629-2645.


