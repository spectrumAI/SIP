# Preprocess raw Sentinel-1 data by just one click:

**Step 1: Download data.** Downlaod Sentinel1 SAR scenes from https://search.asf.alaska.edu/#/.

**Step 2: Copy all downloaded .zip files to /data/sentinel1_raw_zip/ folder.** 

**Step 3: Preprocess.** 
- Run SIP, and click on ***"preprocessing -> sentinel1"***;
- First select the /data/sentinel1_raw_zip/ folder, and then ***select the sentinel1_config_os.yaml file***;
- Take a look at the preprocessed scenes in /data/sentinel1_preprocessed_imgs/ folder;
- Change the 'multilook_num' parameter in the .yaml config file and re-run the program to see what happens. 

**Step 4: Classification.** Go to step 1 in the first example and start from there to finish.

# Preprocess raw Landsat8 L1TP data. 

**Step 1: Download data.** Downlaod Landsat8 L1TP scenes from USGS Earthexplorer https://earthexplorer.usgs.gov. 

**Step 2: Copy all downloaded .tar.gz files to /data/landsat8_raw_zip/ folder.** 

**Step 3: Preprocess.** 
- Run SIP, and click on ***"preprocessing -> Landsat8 L1TP"***;
- First select the /data/landsat8_raw_zip/ folder, and then ***select the landsat8_config_os.yaml file***;
- Take a look at the preprocessed scenes in /data/landsat8_preprocessed_imgs/ folder;
- Change the 'multilook_num' parameter in the .yaml config file and re-run the program to see what happens. 

**Step 4: Classification.** Go to step 1 in the first example and start from there to finish.


# Process RCM SLC CP Winnipeg scene for land cover classification

**Step 1: Download data.** Downlaod Winnipeg RCM SLC CP scene from ftp://ftp.asc-csa.gc.ca/users/OpenData_DonneesOuvertes/pub/RCM/. Copy the data to /data/rcm_raw_data/ folder.

**Step 2: Edit config file** Open config/rcm_slc_cp_config.yaml, and change all dirs to your own dirs.

**Step 3: Open file.** Run SIP, and click on "preprocessing" manu, and click on "RCM SLC CP". ** You need to first select the .safe file in RCM SLC CP data folder, and then select the .yaml file you just edited.

**Step 4: Classification.** Go to step 1 in the first example and start from there to finish.

![](./pics/rcm.png)

![](./pics/rcm_result.png)



