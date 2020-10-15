# Preprocess raw Sentinel-1 data:

**Step 1: Download data.** Downlaod Sentinel1 SAR scenes from https://search.asf.alaska.edu/#/.

**Step 2: Copy all downloaded .zip files to /data/sentinel1_raw_zip/ folder.** 

**Step 3: Modify the config_os.yaml file** to set the ***sentinel1_zip_dir*** to be the folder in step 2, and set the ***output_raw_img_dir*** parameter to the folder where you want to save the preprocessed images. You can also change the other parameters, please refer to [config file](./config_file.md) for how to do it. 
  
**Step 4: Preprocess.** 
- Run SIP, and click on ***"preprocessing -> sentinel1"***;
- First select the /data/sentinel1_raw_zip/ folder, and then ***select the config_os.yaml file***;
- Take a look at the preprocessed scenes in your ***output_raw_img_Dir*** folder;
- Change the 'multilook_num' parameter in the .yaml config file and re-run the program to see what happens. 

**Step 4: Classification.** Go to step 1 in the first example and start from there to finish.

# Preprocess raw Landsat8 L1TP data. 

**Step 1: Download data.** Downlaod Landsat8 L1TP scenes from USGS Earthexplorer https://earthexplorer.usgs.gov. 

**Step 2: Copy all downloaded .tar.gz files to /data/landsat8_raw_zip/ folder.** 

**Step 3: Modify the config_os.yaml file** to set the ***Landsat_zip_dir*** and the ***output_raw_img_dir*** parameters. You can also change the other parameters, please refer to [config file](./config_file.md) for how to do it.

**Setp 4: Modify the config_os.yaml file** to set ***Landsat_data_type*** to ***LC08_L1TP***. Other supported Landsat products include ***LE07_L1TP***, and ***LT05_L1TP***. If you are preprocessing these products, they need to be specified here.  
 
**Step 5: Preprocess.** 
- Run SIP, and click on ***"preprocessing -> Landsat"***;
- First select the /data/landsat8_raw_zip/ folder, and then ***select the config_os.yaml file***;
- Take a look at the preprocessed scenes in ***output_raw_img_dir*** folder;
- Change the 'multilook_num' parameter in the .yaml config file and re-run the program to see what happens. 

**Step 6: Classification.** Go to step 1 in the first example and start from there to finish.

# Preprocess raw MODIS MOD09GA data. 

**Step 1: Download data.** Go to https://ladsweb.modaps.eosdis.nasa.gov/search/ to make an order of MOD09GA scenes you want to download. An email will be forward to you with an url to the dataset you ordered. You will need to copy this url to the config_os.yaml file.   

**Step 2: Modify the config_os.yaml file** to set the ***Modis_url*** parameter to be the url in step 1. You also need to set the ***Modis_token*** parameter to be the token you recieved from the EarthData website.  

**Step 3: Modify the config_os.yaml file** to set the ***Modis_hdf_dir*** to a folder to save the downloaded hdf files, and set the ***output_raw_img_dir*** parameter to a folder to save the preprocessed images. You can also change the other parameters, please refer to [config file](./config_file.md) for how to do it.

**Setp 4: Modify the config_os.yaml file** to set ***Modis_data_type*** to ***MOD09GA***. Other supported Landsat products include ***MOD02QKM***, ***MOD13Q1***, ***MCD64A1*** and ***MCD12Q1***. If you are preprocessing these products, they need to be specified here.  
 
**Step 5: Preprocess.** 
- Run SIP, and click on ***"preprocessing -> Modis"***;
- First select the /data/modis_raw_hdf/ folder, and then ***select the config_os.yaml file***;
- Take a look at the preprocessed scenes in ***output_raw_img_dir*** folder;
- Change the 'multilook_num' parameter in the .yaml config file and re-run the program to see what happens. 

**Step 6: Classification.** Go to step 1 in the first example and start from there to finish.


# Process RCM SLC CP Winnipeg scene for land cover classification

**Step 1: Download data.** Downlaod Winnipeg RCM SLC CP scene from ftp://ftp.asc-csa.gc.ca/users/OpenData_DonneesOuvertes/pub/RCM/. Copy the data to /data/rcm_raw_data/ folder.

**Step 2: Edit config_os.yaml file** to set the ***raw_safe_dir*** to be the folder in step 1, and set the ***output_raw_img_dir*** to save the preprocessed images.

**Step 3: Open file.** Run SIP, and click on "preprocessing" manu, and click on "RCM SLC CP". ** You need to first select the .safe file in RCM SLC CP data folder, and then select the .yaml file you just edited.

**Step 4: Classification.** Go to step 1 in the first example and start from there to finish.

![](./pics/rcm.png)

![](./pics/rcm_result.png)



