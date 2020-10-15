# First example: Process Sentinel-1 SAR image for sea ice classification

![](./pics/classify.gif)

**Step 1: Run app and open data.** 
- Run SIP;
- Open the '.json' file in ***'SIP/data/sentinel1_preprocessed_imgs'***;
- Also open the associated '.tiff' image.

**Step 2: Draw region of interst (ROI).**  
- ***Double click a class*** in the 'Label List' panel on the right to choose a class; 
- Draw point, or line or polygon to add more ROI for this class;
- ***To finish drawing line and polygon, type 'c' from keyboard***;
- Save drawing using default name;

**Step 3: Edit config file.** 
- Copy the ***config_os.yaml.bak*** defined in the config folder and change its name to ***config_os.yaml***;
- Find ***sentinel1_params***, ***raw_data_dir***, ***dirs***; ***For all directories in these three parameters, make sure you have changed them to your own directories***.

**Step 4: Prepare label mask.** 
- Click on ***"Get masks"*** under ***Classification*** menu;
- First select the config file you just edited, and then select the csv file you just saved;
- This step transfer ROIs from vectors to mask images;
- Take a look at the png images generated in the ***raw_img_dir*** directory defined in the config file;

**Step 5: Prepare all dirs and data.** 
- Click on ***"Prepare data"*** under ***Classification*** menu to prepare all training, test and prediction data. 
- You need to choose the .yaml 'config' file you just edited. 
- Once finished, go to ***dirs->data->train/val/test/predict*** folders defined in the config file, to open and take a look at the ***data_file.yaml*** files. Think about why ***dirs->data/predict*** has different image with train, val and test.   

**Step 6: Train classifier.** 
- Click on ***'Train classifier'*** under ***Classification*** menu and then choose the .yaml config file you just edited. 
- Once training is finished, go to ***raw_data_dir*** defined in the config file to take a look at the generated label maps of the training images. 
- Go to ***dirs->save->model*** folder defined in the config file, check ***the training and validation accuracies*** in the ***train.log*** file.

**Step 7: Test classifier.** 
- You can optionally run ***"Test classifier"*** under the ***Classification***, but it will run on the same image using the trained model in Step 6. 
- You also need to select the same .yaml config file. 
- Check the ***test accuracies*** in the "test.log" file under the ***dirs->save->model*** folder defined in the config file.  

**Step 8: Predict label map on a new image.** 
- Click on ***"Predict image"*** under the ***Classification*** menu to run the trained model on the other scene in the ***raw_img_dir*** folder. You also need to select the same config file. 
- Once it is done, you can check the label map of the test image in the ***raw_img_dir*** folder.
- Check the "predict.log" file under the ***dirs->save->model*** folder defined in the config file. 


