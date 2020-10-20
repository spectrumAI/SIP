# Combining pixel-based and image-based convolutional neural networks (CNNs) for sea ice mapping from Sentinel-1 SAR images. 

## Tutorial Objectives

As the first step, this tutorial has the following objectives:

* ***Compare the performance of two typical CNN architectures***, i.e., (1) pixel/patch-based CNN architecture that outputs
the label of a single pixel, and (2) image-based fully convolutional neural (FCN) network architecture that outputs a label map of 
all pixels on the image. 

    * Both approaches are residual neural network;
    * Approach (1) tends to have a smaller field of view than approach (2) that 
has many convolutional and pooling layers, and therefore may be less efficient at texture feature extraction;
    * If the input image patch is big, approach (1) is less efficient at preserving small objects and class boundaries,
whereas approach (2) can do a better job using some detail-perservation tricks, e.g., skip connnections;
    * Due to patch-overlapping, approach (1) requires more GPU memories during training;
    * Approach (1) is trained on pixel samples which are relatively easier to obtain with a large quantity, 
whereas approach 2 is trained on much less image samples whose full label maps are difficult to obtain;

* ***Combine approach (1) and (2) to develop a semi-supervised classification approach*** (3).

Table 1. The comparison of three approaches.

&nbsp; | Pixel/patch-based CNN | Image-based FCN | Combined approach
------------- | ------------ | ------------- | -------------
Backbone | ResNet | ResNet | Resnet
Field of view | small | big if many Conv layers | big if many Conv layers
Detail preservation capability | weak | stronger | stronger
Computational efficiency | low | higher | higher during prediciton
GPU memory consumption | large | smaller | smaller
Training samples | many training pixels | sparse label maps | ***full label maps***


## Experiments

1. ***Experiment with approach 1***

   1. Train approach (1) using pixels sampels collected from the training images;
   1. Use the trained (1) to generate full label maps for training and test images;
   1. Calculate training, validation and test accuracies for approach (1)

1. ***Experiment with approach 2***

    1. Train approach (2) using image samples whose label maps are sparse in the sense that only 
some pixels on these maps have labels, and most pixels do not have labels;
    1. Use the trained (2) to generate full label maps for training and test images;
    1. Calculate training, validation and test accuracies for approach (2)

1. ***Experiment with approach 3***

    1. Train approach (2) using generated full label maps of the training images generated in experiment 1 step ii;
    1. Use the trained (2) to predict full label maps of training images, validation images and test images; 
    1. Calculate the training, validation and test accuracies of this approach, which is approach 3;
    1. Compare the accuracies of approach 1, 2, and 3 to see whether approach 3 has the highest accurcies;

## Procedures of Experiment 1
 
1. **Edit config file.** 
    1. Copy the ***config_os.yaml.bak*** in the config folder and change its name to ***config_os.yaml***;
    1. Find ***sentinel1_params***, ***raw_data_dir***, ***dirs***; ***For all directories in these three parameters, make sure you have changed them to your own directories***.
    1. Make sure the ***train_params -> net_type*** is ***ss_res***. 
 
1. **Download data.**
    1. Go to https://search.asf.alaska.edu/#/. Please copy *POLYGON((-169.5982 69.4421,-157.2591 71.9421,-141.6889 70.0305,-135.3576 69.9672,-127.5021 71.384,-139.323 78.3118,-164.763 80.1023,-169.5982 69.4421))* and paste it to **Area of Interet** in the webpage. Click ***Filters*** and set ***starting date*** and ***end date*** to be 2019.09.01, ***File type*** to be L1 GRD MD, and ***Beam Mode*** to be EW. Click on ***Search***, and you should find 10 scenes. Download them. 
    1. Copy all files to the ***sentinel1_zip_dir*** folder defined in the ***config_os.yaml*** file. 

1. **Preprocess data.**
    1. Run SIP.
    1. Click on ***Preprocessing -> Sentinel1***, and then select the folder with sentinel1 zip files, and then select the ***config_os.yaml*** file you edited. 
    1. Go to ***sentinel1_params -> output_raw_img_dir*** defined in the ***config_os.yaml*** file to take a look at the preprocessed images. 

1. **For each scene, do the following steps to make sure it is very well classified**
    1. **Copy the HH and HV images of one scene**
        1. Make sure that the ***raw_data_params -> raw_img_dir*** is different with ***sentinel1_params -> output_raw_img_dir***.
        1. Copy the HH and HV images of one scene to the ***raw_data_params -> raw_img_dir*** directory, and make sure that this directory contains only this scene. 
    1. **Draw ground truth samples.**  
        1. Click on ***Open an image*** button to select a HV channel image in the ***raw_data_params -> raw_img_dir*** directory and open it. 
        1. Click on ***Edit class labels"*** button, and type 4 in the ***Initialize labels*** dialog, and click ***ok***. In the next dialog, ***make sure*** you set the name of the 4 classes to be ***ice_train, ice_val, water_train, water_val*** respectively, and also specify different colours for them, click ***ok***. 
        1. Double click the ***ice_train*** item in the 'Label List' panel on the right to draw training samples for the ice class. Click on the ***Draw line*** button to draw lines on the ice covered area in the image. ***To finish drawing line and polygon, type 'c' from keyboard***. You can also ***Draw point***, or ***Draw polygon***. 
        1. ***Remember to frequently save your results using the default file name.*** 
        1. Similarly, double click on ***ice_val*** to draw validation samples for the ice class, and ***water_train***, and ***water_val*** for training and validation samples of the water class. 

    1. **Prepare label mask.** 
        1. Once sample drawing is finished and saved, click on ***"Get masks"*** under ***Classification*** menu;
        1. First select the config_os.yaml file you edited, and then select the csv file you just saved;
        1. This step transfer ROIs from vectors to mask images;
        1. Take a look at the png images generated in the ***raw_img_dir*** directory defined in the config file;

    1. **Prepare all dirs and data.** 
        1. Click on ***"Prepare data"*** under ***Classification*** menu to prepare all training, validation, test and prediction data. 
        1. You need to choose the ***config_os.yaml*** file.
        1. Once finished, go to ***dirs->data->train/val/test/predict*** folders defined in the config file, to open and take a look at the ***data_file.yaml*** files.

    1. **Train classifier.** 
        1. Click on ***'Train classifier'*** under ***Classification*** menu and then choose the config_os.yaml file. 
        1. Once training is finished, go to ***raw_data_dir*** defined in the config file to take a look at the generated label maps of the training images. 
        1. Go to ***dirs->save->model*** folder defined in the config file, check ***the training and validation accuracies*** in the ***train.log*** file.

    1. **Is the result good?**
        1. If the validation accuracy and training accuracy is over 90%, and the classification map looks really good with no apparent misclassification, you can move on to process the next scene using the above steps. 
        1. However, if there are apparent misclassifion on the label map, you need to draw more training samples on the misclassified areas and retrain the classifier to reduce these misclassifications. Keep repeating the above steps until the label map is acceptable. 

    1. **Save the data of the processed scene**
        1. If the result of this scene is good, rename the ***all_data*** folder to ***all_data_sceneID***, with ***sceneID*** being the ID string of the scene that differentiate it from the other scenes. Copy all the results in ***raw_data_params -> raw_img_dir*** folder to a different folder named ***sentinel1_gt***.

1. **Decide which scenes are used for training and which are used for testing**
    1. Change ***raw_data_params -> raw_img_dir*** in the ***config_os.yaml*** file to the ***sentinel1_gt*** folder, where you should have the ***.csv*** and ***.json*** files for all scenes. 
    1. Randomly select about half of all the scenes for training and the left scenes for testing. 
    1. For the test scenes, please open them one by one in SIP and combine the ***ice_train*** and ***ice_val*** classes into the ***ice_test*** class, and combine the ***water_train*** and ***water_val*** classes into the ***water_test*** class. 
        1. Click on the ***Edit class labels*** button to add 2 more classes with names ***ice_test*** and ***water_test*** respectively, and also specify colours for them. 
        2. Click on the ***Edit drawing*** button, move your mouse above the drawing you want to edit, when the line colour turn white, right-click the line and select the ***ice_test*** or ***water_test*** class from the ***Line*** dropdown list. ***Remember to save the results.*** 
        3. Alternatively, you can open the ***.csv*** and ***.json*** file associated with the scene and change the ***ice_train*** and ***ice_val*** into ***ice_test***, and change the ***water_train*** and ***water_val*** into ***water_test***. 

1. **Train classifier using all training scenes**
    1. **Prepare label mask.** 
        1. Click on ***"Get masks"*** under ***Classification*** menu;
        1. First select the config_os.yaml file, and then select the csv file you just saved;
        1. This step transfer ROIs from vectors to mask images;
        1. Take a look at the png images generated in the ***raw_img_dir*** directory defined in the config file;

    1. **Prepare all dirs and data.** 
        1. Click on ***"Prepare data"*** under ***Classification*** menu to prepare all training, validation, test and prediction data. 
        1. You need to choose the config_os.yaml file. 
        1. Once finished, go to ***dirs->data->train/val/test/predict*** folders defined in the config file, to open and take a look at the ***data_file.yaml*** files.

    1. **Train classifier.** 
        1. Click on ***'Train classifier'*** under ***Classification*** menu and then choose the config_os.yaml file. 
        1. Once training is finished, go to ***raw_data_dir*** defined in the config file to take a look at the generated label maps of the training images. 
        1. Go to ***dirs->save->model*** folder defined in the config file, check ***the training and validation accuracies*** in the ***train.log*** file.

1. **Test classifier on all testing scenes** 
    1. Run ***"Test classifier"*** under ***Classification***. It will test the trained classifier on all test scenes. 
    1. Check the ***test accuracies*** in the "test.log" file under the ***dirs->save->model*** folder defined in the config file.  
    1. Compare the test accuracies with the training accuracies and validations accuracies in the previous step. 
    1. Take a look at the label maps of all the testing scenes. 


## Procedures of Experiment 2

1. **Change the config file**
    1. Open the ***config_os.yaml*** file and change the ***train_params -> net_type*** to ***dip_res***. 
    1. In the config file, also search ***batch_size_train***, ***batch_size_val***, ***batch_size_test***, and change their values to be 1. 
    1. Change ***lr*** in the config file to be ***0.01***. 
    1. Change ***var*** in the config file to be ***0.1***. 

1. **Train and test the classifier using the training and test scenes**
    1. **Train classifier.** 
        1. Click on ***'Train classifier'*** under ***Classification*** menu and then choose the config_os.yaml file. 
        1. Once training is finished, go to ***raw_data_dir*** defined in the config file to take a look at the generated label maps of the training images. 
        1. Go to ***dirs->save->model*** folder defined in the config file, check ***the training and validation accuracies*** in the ***train.log*** file.
        1. Go to ***dirs->save->model*** folder defined in the config file, check the label map over different epochs. If you do not want to generate these maps during iteration, you can set ***save_map_over_epoch*** in the config file to be False.  
    1. **Test classifier on all testing scenes** 
        1. Run ***"Test classifier"*** under ***Classification***. It will test the trained classifier on all test scenes. 
        1. Check the ***test accuracies*** in the "test.log" file under the ***dirs->save->model*** folder defined in the config file.  
        1. Compare the test accuracies with the training accuracies and validations accuracies in the previous step. 
        1. Take a look at the label maps of all the testing scenes. 


1. **Compare the test accuracies with the final test accuracies in Experiment 1**

## Procedures of Experiment 3

1. **Make sure the config file is the same with Experiment 2**

1. **Use the predicted label maps as ground truth**
    1. Cut the training, validation and test masks for all the scenes in the ***raw_data_params -> raw_data_dir*** folder, and paste them to a different folder. 
    1. Rename the labels maps produced by the classifier in Experiment 2. For each scene, make sure that the new name of the label map is the same with the cutted training, validation or test masks in the previous step. 

1. **Train and test the classifier using the training and test scenes**
    1. The training procedures are similar with the final training procdures in Experiment 2. 
    1. The test procedures are similar with the final test procedures in Experiment 2. 

1. **The test accuracies here should be higher than those in Experiment 1 and Experiment 2**


