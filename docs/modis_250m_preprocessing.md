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

## Procedures of this experiment

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
