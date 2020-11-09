# Mapping of different ice types in Arctic ocean using Sentinel-1 images

## Tutorial Objectives

* Explore the feasibility of using ***deep learning for discriminating different ice types***.

## Deep learning models

* Approach (1): Pixel/patch-based CNN algorithm

## Key challenges

* Big ***signature variabilities*** of different sea ice types due to large spatial extent, banding effect in HV channel, incidence angle effect in HH channel

## Experiments

1. ***Experiment 1: Preprocess and mosaic Sentinel-1 scenes on Feb. 8, 2019***

    1. Download Sentinel-1 images ***covering the entire Arctic ocean on Feb. 8, 2019***; 
    1. Preprocess downloaded scenes to generate a big mosaic; 
 
1. ***Experiment 2: Download and examine the ground truth ice chart***

    1. Download the ice chart covering the Arctic ocean on Feb. 8, 2019. 
    1. Open ice chart and the mosaic image in QGIS software to identify different ice types. 

1. ***Experiment 3: Draw training and validation samples in SIP and perform classification***

    1. Open mosaic HV image in SIP, and draw training and validation samples for different ice types;
    1. Perform model training;
    1. Record the training and validation accuracies;
    1. Generate ***map for the entire Arctic ocean on Feb. 8, 2019***;

1. ***Experiment 4: Test the trained model on Feb. 7, 2020***

    1. Download Sentinel-1 images covering the entire Arctic ocean on ***Feb. 7***, 2020;
    1. Download ice chart on Feb. 7, 2020. 
    1. Preprocess and mosaic the scenes as in Experiment 1;
    1. Draw test samples according to ice chart;
    1. Use the trained model in Experiment 3 to predict the label maps of mosaic HH and HV images;
    1. Calculate test accuracies, and compare with training and validation accuracies in Experiment 3;
    1. Generate ***map for the entire Arctic ocean on Feb. 7, 2020***;

## Procedures of Experiment 1

* **Download data.** 
    1. Go to https://search.asf.alaska.edu/#/. Click on ***Arctic map view*** under the ***Map projection*** panel. Please copy *POLYGON((162.1385 70.5747,170.3789 69.1826,173.0147 69.9109,-171.9991 66.2364,-165.3146 66.5971,-163.8295 70.5517,-141.0027 70.2103,-131.8234 69.4745,-118.7329 69.2591,-125.7601 72.4353,-123.7074 76.5085,-109.8374 80.0968,-90.9058 82.533,-66.0815 84.1377,-58.0219 83.4228,-18.0914 83.7729,12.2722 80.3598,31.7989 81.3237,61.4568 81.9729,88.5461 81.2813,98.6308 79.982,112.9429 76.1546,116.1535 73.5502,134.7756 74.093,130.9475 72.1052,138.6538 73.7119,133.8407 77.6527,153.1613 74.8675,151.2038 72.315,162.1385 70.5747))* and paste it to **Area of Interet** in the webpage. Click ***Filters*** and set ***starting date*** and ***end date*** to be 2019.09.01, ***File type*** to be L1 GRD MD, and ***Beam Mode*** to be EW. Click on ***Search***, and you should find 59 scenes. Download them. 

* **Preprocess and Mosaic all downloaded scenes.**
    1. Copy ***config_os.yaml.bak*** to ***config_os.yaml*** and change all directories to your own directories.
    1. Copy all downloaed zip files to the ***input_dir_unzip_s1*** folder defined in the ***config_os.yaml*** file. 
    1. Run SIP software, and click on the ***Sentinel1*** item under the ***Preprocessing*** menu. 
    1. Select the ***input_dir_unzip_s1*** folder first and then select the ***config_os.yaml*** file. 
    1. SIP will sequentially run ***(1) Preprocessing (including denoising and multilooking)***, ***(2) Geocoding and reprojecting Geotiff to Polar mapping system***, ***(3) Performing land masking using coastline shapefile*** and ***(4) Mosaicing all the scenes in the big images***. 
    1. Once it finished, go to ***output_dir_merge_s1*** folder defined in ***config_os.yaml*** to make sure that there are three mosaic images, i.e., ***HH, HV and background_mask***. 
    1. Open the mosaiced ***HH, HV and background_mask*** images in QGIS to take a look. You can also open the ***coastline.shp*** file under ***SIP/data/coastline_shp*** folder to see whether the mosaics align well with the coastline. 

## Procedures of Experiment 2

    1. Go to https://www.natice.noaa.gov/products/weekly_products.html, and dwonload the ***arctic190208.zip*** shapefile;
    1. Open in QGIS also ***mosaic HH, HV*** images generated in Experiment 1;
    1. Open in QGIS the ***coastline.shp*** in ***SIP/data/coastline_shp*** folder; 
    1. Unzip the arctic190208.zip and open the ***.shp*** file in QGIS;
    1. Now, you see many polygons of the same color. Each polygon contains some ice classes. ***How do you know which classes are contained in a polygon?*** Click the ***Identify Features*** button, and then click on a polygon. Then, in QGIS, you will see features of this polygon, e.g., CT, CA, CB, etc. ***What does these symbols mean?*** ***CT=81*** means 81% of the pixels in this polygon is covered by ice. ***CA=70, CB=20, CC=10*** means the ***thickest*** ice in this polygon has a percentage of 70%, the ***second thickest*** takes 20%, and the ***third thickest*** takes 10%. ***How do I know ice types of these CA, CB and CC?*** That is by looking at ***SA, SB and SC*** respectively. If ***SA=95***, then ***CA*** is ***old ice***. If ***SA=85***, then ***CA*** is ***Grey-White ice***. ***How do I interpret these code, e.g., 95, 85?***, See Table 4.2 in https://library.wmo.int/doc_num.php?explnum_id=9270.  
    1. If ***SA=old_ice*** but ***SB=first_year_ice***, then, ***does this polygon belong to old_ice or first_year_ice***? It is by looking at values of ***CA and CB***. If ***CA>CB***, then there are more old ice than first year ice, and so it make sense to assign the whole polygon to be old ice, if you can only choose one class. You need to determine the class label for each polygon according to this rule. 

    1. Now, all polygons should appear the same color. To display different colors for different ice types, you need to display the ***SA*** field in the properties table. You can display it according by following this tutorial: http://www.qgistutorials.com/fr/docs/basic_vector_styling.html
    1. Suppose you now show different ***SA*** as different colors. You need to keep in mind that ***you cannot trust these colors as the ice types of different polygons***, because you have to use ***SB*** instead of ***SA***, if ***CA<CB***. You need to flip between the ice chart layer and the HV mosaic image layer to see what different ice types looks like. 
    1. Six different classes (***open water, new ice, young ice, grey ice, first-year ice, old ice***) will be classified in SIP. So, you need to remember where these six classes are, to draw traning and validation samples in SIP. In ice chart, there are ***many different first-year ice types***. Here, different first year ice types in ice chart are treated as being one class, i.e., first-year ice, and Grey-white ice and grey ice are combined into grey ice, as in Reference 1 and 2 below.  

## Procedures of Experiment 3

    1. Based on the information you see in QGIS about the six classes, draw training and validations samples for these six classes. For each class, you need to define ***train*** and ***val***. So, you will need to draw samples for 12 groups, i.e., ***old_train, old_val, first_train, first_val, young_train, young_val, grey_train, grey_val, new_train, new_val, water_train, water_val***. 

    1. Open ***config_os.yaml***. Make sure the number of classes to be 6, and  names of the classes are ***old, first, young, grey, new, and water***.

    1. Go to the previous tutorials to see how to draw training and validation samples, and how to perform model training.

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
 
## References

1. Boulze, Hugo, Anton Korosov, and Julien Brajard. "Classification of sea ice types in Sentinel-1 SAR data using convolutional neural networks." Remote Sensing 12.13 (2020): 2165.

2. Park, Jeong-Won, et al. "Classification of sea ice types in Sentinel-1 synthetic aperture radar images." The Cryosphere 14.8 (2020): 2629-2645.


