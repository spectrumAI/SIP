# Modis 250m land products MOD13Q1 preprocessing

## Tutorial Objectives

* Explore the data preprocessing pipeline for MOD13Q1 products that can optimize the subsequent LCLU classification process using deep learning approaches. 
* Generate 250m Modis time series that covers whole Canada.  

## MOD13Q1 Preprocessing Steps

1. Downloading
1. Transform hdf format to geotiff format
1. Mosaic geotiff files into a single large file
1. Reproject the merged large file
1. Clip the reprojected file to generate a smaller image of the study area
1. Extract different bands in the clipped image and save them into separate images
1. Split the band into small blocks for training fully covolutional neural networks

## Experiment 1: Generate 250m NDVI time series covers Canada

* **Download data.** 
    1. Go to https://ladsweb.modaps.eosdis.nasa.gov/ to register an account, and then click the ***Find Data*** button to open the data downloading page, where specify the ***PRODUCTS*** to be ***MOD13Q1***, and ***TIME*** to be from ***2010-01-01*** to ***2010-12-31***, ***LOCATION*** to be ***Canada*** using the ***Countries*** selection tool on the right, ***FILES*** to ***select all*** the files, ***REVIEW & ORDER*** to ***Submit Order***. There should be a total of 224 files.  
    1. Open your email you received from LAADS and copy the download link and past it to ***Modis_url*** in your ***config_os.yaml*** file. Also, you need to specify your ***Modis_token*** using your EOSDIS account information, spefify ***Modis_hdf_dir***, and set ***to_download_Modis*** to be ***True*** in the config file. 
    1. Disable all the other operations by setting the ***to_XXX_Modis*** to be ***False***.
    1. Run SIP software and click ***Preprocessing -> Modis*** to download the data

* **Modify config file**
    1. Under ***Modis_params*** in your ***config_os.yaml*** file, make sure you change all directories to your own directories. 
    1. Make sure you enable all preprocessing steps by setting ***to_XXX_Modis*** to be ***True***. 
    1. Make sure you disable ***Modis_maps_merge_params*** by setting ***to_merge_classification_maps_Modis*** to be ***False*** because you do not need this step during preprocessing. 
    1. Make sure you unzip the ***canada_lc_classification.zip*** file in the ***SIP/data*** folder and use it to set all relevant parameters.
    1. Set ***to_download_Modis*** to be False.

* **Run preprocessing**
    1. Run SIP software and click on ***Preprocessing -> Modis*** to conduct step-by-step processing. 
    1. Once it finished, go the output folders to take a look at the generated images and open them in QGIS software check whether they have been geocoded correctly. 

## Experiment 2: Generate 250m NDVI time series for the Saskatchewan province in Canada

* **Finish Experiment 1**
    1. Make sure you finished experiment 1. And, based on ***config_os.yaml*** in Experiment 1, finish the following steps. 

* **Modify config file** 
    1. Open ***config_os.yaml***.
    1. Under ***Modis_params***, ***enable clipping*** by setting ***to_clip_Modis*** to be ***True***. Set ***input_dir_clip_Modis*** to be ***MOD13Q1_raw_hdf/clip*** where you generate the clipped tiffs files in Experiment 1. Set ***output_dir_clip_Modis*** to be ***MOD13Q1_raw_hdf/clip_saskatchewan***. Set ***Clip_Modis_id_str*** to be ***'\*-merged-reprojected-clipped.tiff'***. 
    1. Unzip ***canada_lc_classification.zip***, make sure you use ***clipper_sask.shp***, ***label_canada_250m_sask.tif*** and ***canada_mask_sask.tiff*** to reset ***bounding_box_XXX*** and ***Modis_label_map_file*** parameters in ***config_os.yaml***. 
    1. ***Enable generating small blocks*** by setting ***to_generate_small_blocks_Modis*** to be ***True***. Set ***input_dir_small_blocks_Modis*** to be ***MOD13Q1_raw_hdf/clip_saskatchewan***, and ***output_dir_small_blocks_Modis*** to be ***MOD13Q1_preprocessed_imgs_saskatchewan***. Set ***generate_small_blocsk_Modis_id_str*** to be ***'\*-clipped-clipped.tiff'***. Set ***Modis_label_map_file*** to be ***label_canada_250m_sask.tif***. 
    1. Enable ***post processing small blocks*** by setting ***to_post_process_small_blocks_Modis*** to be ***True***. 
    1. Disable ***all the other processing steps*** by setting ***to_XXXXX_Modis*** to be ***False***. 

* **Run preprocessing**
    1. Run SIP software and click on ***Preprocessing -> Modis*** to conduct clipping and small blocks processing. 
    1. Once it finished, go to the ***MOD13Q1_preprocessed_imgs_saskatchewan*** folder to take a look at the generated images and open them in QGIS to make sure they have been geocoded correctly. 

## Experiment 3: Generate 250m NDVI time series for the Alberta province in Canada

* **Repeat Experiment 2, but using Alberta data**
    1. Repeat all steps in Experiment 2. The only difference is that make sure you use ***clipper_alta.shp***, ***label_canada_250m_alta.tif*** and ***canada_mask_alta.tiff*** to reset ***bounding_box_XXX*** and ***Modis_label_map_file*** parameters in ***config_os.yaml***. 
 

## Experiment 4: Generate 250m EVI time series for the Saskatchewan province in Canada

* **Finish Experiment 2**
    1. Make sure you finished experiment 2. And, based on ***config_os.yaml*** in Experiment 2, finish the following steps. 

* **Modify config file**
    1. Open ***config_os.yaml***.
    1. Under ***Modis_generate_small_blocks_params***, change ***used_bands_indices*** to be ***1***. Set ***output_dir_small_blocks_Modis*** to be ***MOD13Q1_preprocessed_imgs_saskatchewan_evi***. 
    1. Disable ***clipping operation*** by setting ***to_clip_Modis*** to be ***False***. 

* **Run preprocessing**
    1. Run SIP software and click on ***Preprocessing -> Modis*** to conduct clipping and small blocks processing. 
    1. Once it finished, go to the ***MOD13Q1_preprocessed_imgs_saskatchewan*** folder to take a look at the generated images and open them in QGIS to make sure they have been geocoded correctly.

## Experiment 5: Generate 250m Reflectance channels for the Saskatchewan proviince in Canada

* **Repeat Experiment 4, but use different channel**
    1. Repeat Experiment 4. But, change ***used_bands_indices*** to be 2, 3, 4, 5, for respectively the red, near infrared, blue and medium infrared channels. And, change ***output_dir_small_blocks_Modis***.  
    1. Instead of just using one reflectance channel, you can use multiple channels by setting ***used_bands_indices*** to consists of more than one values. In this case, the final time series will consist of the time series of multiple channels. 

 

 
