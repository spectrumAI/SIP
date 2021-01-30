# First example: Process Sentinel-1 SAR image for sea ice classification

![](./pics/classify.gif)

**Step 1: Edit config file.** 
- Copy the ***config_os.yaml.bak*** defined in the config folder and change its name to ***config_linux.yaml*** or ***config_win.yaml*** depending on your operational system;
- Find ***raw_data_dir***, ***dirs***; ***For all directories in these three parameters, make sure you have changed them to your own directories***. Specifically, change all ***/my_dir/sentinel1_preprocessed_imgs/*** to ***/your_SIP_install_dir/data/sentinel1_preprocessed_imgs/***.

**Step 2: Run app and open data.** 
- Run SIP;
- Open the 'multilook-4-s1a-ew-grd-20150915t180234-20150915t180338-007729-00abc9_hv-002.tiff' file in ***raw_img_dir*** folder defined in the ***config_os.yaml*** file;
- Click ***yes*** on the ***Label file*** dialogy, and open the default '.json' file;
- You will see images topped with lines of different colors for the ***ice*** and ***water*** class in respectively the ***training***, ***validation*** and ***test*** sets. 

**Step 2: Draw region of interst (ROI).**  
- ***Double click a class*** in the 'Label List' panel on the right to choose a class; 
- Draw point, or line or polygon to add more ROI for this class;
- ***To finish drawing line and polygon, type 'c' from keyboard***;
- Save drawing using default name;

**Step 4: Prepare label mask.** 
- Click on ***"Get masks"*** under the ***Classification*** menu;
- Select the ***config_os.yaml*** file you just edited, and then select the ***default csv file*** you just saved;
- This step transfer ROIs from vectors to mask images;
- Take a look at the png images generated in the ***raw_img_dir*** directory defined in the config file;

**Step 5: Prepare all dirs and data.** 
- Click on ***"Prepare data"*** under the ***Classification*** menu to prepare all training, test and prediction data. 
- You need to choose the ***config_os.yaml*** file. 
- Once finished, go to ***dirs->data->train/val/test/predict*** folders defined in the config file, to open and take a look at the ***data_file.yaml*** files. Think about why ***dirs->data/predict*** has different image with train, val and test.   

**Step 6: Train classifier.** 
- Click on ***'Train classifier'*** under the ***Classification*** menu and then choose the ***config_os.yaml*** file. 
- Once training is finished, go to ***raw_data_dir*** defined in ***config_os.yaml***  to take a look at the generated label maps of the training images. 
- Go to ***dirs->save->model*** folder defined in ***config_os.yaml***, check ***the training and validation accuracies*** in the ***modelName-trainTime-train.log*** file.

**Step 7: Test classifier.** 
- You can optionally run ***"Test classifier"*** under ***Classification***. Select the ***config_os.yaml*** file. 
- It will predict the image and calculate test accuracy using the ***test samples*** defined in the ***.json*** file. 
- Check the ***test accuracies*** in the "modelName-testTime-test.log" file under the ***dirs->save->model*** folder defined in ***config_os.yaml***.  

**Step 8: Predict label map on a new image.** 
- Click on ***"Predict image"*** under ***Classification*** to run the trained model on the other scene in the ***raw_img_dir*** folder. You also need to select ***config_os.yaml***. 
- Once it is done, you can check the label map of the test image in the ***raw_img_dir*** folder.
- Check the "modelName-testTime-predict.log" file under the ***dirs->save->model*** folder defined in ***config_os.yaml***. 


