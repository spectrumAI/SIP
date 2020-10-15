
# 1. Preprocessing raw data of different sensors

SIP supports the preprocessing of raw data to extract enhanced images and features as input to the classification algorithms. Parameters are illustrated using landsat8 as an example.  

```
Landsat_params:
    Landsat_zip_dir: /home/l44xu/SIP/data/landsat8_raw_zip
    raw_Landsat_format: tar.gz
    Landsat_data_type: LC08_L1TP #LT05_L1TP #LE07_L1TP LC08_L1TP

    to_unzip: True
    multilook_num: 1
    low_percentage_to_remove: 0.02
    high_percentage_to_remove: 0.02

    value_cut_params:
        to_calculate_cut_values: True
        to_use_cut_values: True
        min_value_cut: 5000
        max_value_cut: 40000

    output_raw_img_dir: /home/l44xu/SIP/data/landsat8_preprocessed_imgs_usa/
```
**1.1 Support different Landsat products**, including ***LC08_L1TP***, ***LE07_L1TP***, and ***LT05_L1TP***. You need to define ***Landsat_data_type*** using one of the above product names.  

**1.2 Unzip the raw data.** If ***to_unzip*** is True, the raw data files with extension ***raw_landsat8_format*** in folder ***landsat8_zip_dir*** will be extracted into separate folders. 

**1.3 Multilooking to reduce size.** To reduce image size, a image patch of size ***multilook_num*** by ***multilook_num*** will be averaged into a single pixel in the preprocessed images. 

**1.4 Remove extrame pixels.** To increase image contrast for better visualization, low intensity pixels of percentage ***low_percentage_to_remove*** will be set to the value 0, and high intensity pixels of percentage ***high_percentage_to_remove*** will be set to the value 255. After this, the dynamic range of image will be extended. 

**1.5 Another way to remove extrame pixels*** is by using the ***min_value_cut*** and ***max_value_cut*** parameters. The percentage values calculated for different images will change from image to image, and may cause different images look very different after the enhancement. The cut values here ensure all scenes are enhanced according to the same ***ruler*** and thereby the same class in the image will still look similar after enhancement. To use the cut values instead of the percentage values, you need to set ***to_use_cut_values** to be True. And, instead of manually set the cut values, you can also let the algorithm to automatically calculate and fill these values for you by set ***to_calculate_cut_values*** to be True. 
 
**1.6 Save preprocessed images.** The preprocessed images will be saved into folder ***output_raw_img_dir***.
 
# 2. Preparing data 

SIP automatically prepares the data for classification. No manual operatoins are needed. 

```
raw_data_params:
    raw_img_dir: /home/l44xu/SIP/data/landsat8_preprocessed_imgs
    raw_img_type: tiff
    is_geotiff: True
    raw_id_str_range: #from 'start' to 'end' in the name of the raw image are the string that differenciate different scenes
        start: 10
        end: 34
    raw_band_names: # each raw scene contrain the following bands, each of which is a seperate image in 'raw_img_type' format
        - B2
        - B3
        - B4
```

**2.1 Define the folder of your images** In ***raw_img_dir***, you have multiple scenes, each scene may have multiple bands/channels covering the same location/target. All bands/channels have the format of ***raw_img_type***.

**2.2 How does SIP discriminate different scenes.** To tell SIP about this, you need to make sure that the file name of different scenes within range ***raw_id_str_range*** are different. For example, if ***start*** is 1 and ***end*** is 8, then 20200101_B2.tiff and 20200505_B3.tiff are two different scenes. But, 20200101_B2.tiff and 20200101_B3.tiff are of the same scene. 

**2.3 How does SIP know which bands/channels you want to use.** To tell SIP about this, you need to specify ***raw_band_names*** with different ID names, and make sure your images contain these ID names. In the above code, for each scene, three images whose file name contrain respectively ***B2***, ***B3***, and ***B4*** will be used. 
   
**2.4 Use geotiff.** If ***is_geotiff*** is True, the projection and geotransform information will be extracted and saved into all extracted features when preparing the data. So, the preprocessed images will be geocoded and saved as geotiff format. And, the classification maps will also be saved as geotiff format if ***to_geocode_classification_map*** equals to True.  

# 3. Copy the prepared data to different folders

SIP automatically generates folders and copies data to separate folders for different data processing stages, i.e., ***train***, ***validation***, ***test*** and ***prediction*** stages. 

* **train** folders contain ***data***, ***mask*** and ***result*** for training the classifier.

* **val** folders contain ***data***, ***mask*** and ***result*** for validating the classifier during training or model selection. 

* **test** folders contain ***data***, ***mask*** and ***result*** for testing the accuracies of the classifiers. 

* **prediction** folders contain ***data***, ***mask*** and ***result*** for predicting new images once the model is finalized. ***prediction*** images do not have ground truth masks, but ***test*** images must have ground truth masks to estimate accuracies. 

For each stage, i.e., train, validation, test and prediction, there are ***data***, ***mask*** and ***save*** folders. 

* **data** folders contain prepared bands/channels for each scene. The file format of each scene is defined by ***image_type***. Each scene has a file in the data folder. 

* **mask** folders contain ground truth mask files. The file format is defined in ***mask_type***. Now SIP only supports the ***png*** format.

* **save** folders contain all results generated by SIP classifiers. 

```
dirs:
    image_type: npz
    mask_type: png

    # A (image, mask) pair is identified by looking for the same file name in the 'data' subfolder and 'mask' subfolder
    # E.g., 'hsi1.npz' in /data/train/ is associated with 'hsi1.npz' in /mask/train/ folder
    # ----------------------------------------------------------------
    # (1) config image directories
    
    data: 
    
        # (1.1) training_data_dir contains images with GT in training_mask_dir for training
        train: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/data/train
        
        # (1.2) val_data_dir contains images with GT in val_data_dir for validation
        val: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/data/val
        
        # (1.3) test_data_dir contains images with GT in test_mask_dir for test to acquire training accuracies
        test: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/data/test
        
        # (1.4) predict_data_dir contains images that do not have GT for prediction to acquire classification maps
        predict: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/data/predict
        
    #-----------------------------------------------------------------
    # (2) config GT directories
    
    mask:   
     
        # (2.1) training_mask_dir contrains one-to-one matching masks with images in training_data_dir
        train: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/mask/train
        
        # (2.2) val_mask_dir contains one-to-one matching masks with images in val_data_dir
        val: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/mask/val
        
        # (2.3) test_mask_dir contains one-to-one matching masks with images in test_data_dir
        test: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/mask/test

        # (2.4) predict_mask_dir contains the background pixel masks for some images in predict_data_dir
        predict: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/mask/predict
        
    #------------------------------------------------------------------
    # (3) config results directories
    
    save:
        
        # (3.1) specify directory to save model i.e. best model log file
        model: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/save/model
        
        # (3.2) specify directory to save training results i.e. maps accuracies
        train: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/save/train
        
        # (3.3) specify directory to save val results i.e. maps accuracies
        val: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/save/val
        
        # (3.4) specify directory to save test results i.e. maps accuracies
        test: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/save/test
        
        # (3.5) specify directory to save predict results i.e. maps
        predict: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/save/predict
 
``` 

# 4. Define classes and colors for classification maps
 
SIP allows the user to specify the number of classes, class names and colors. 

```
classification_map_params:

    num_classes: 2
    
    class_color_config_params:
        my_colors:
            - white # color of the background, e.g., land area in sea ice classification
            - yellow # color of the first class
            - indigo # color of the second class
        my_classes: # the class names you defined here have to be consistent with the class names appeared in GUI
                    # e.g., in GUI, if you have land_train, land_val, land_test, water_train, water_val, water_test, 
                    # then, here you need to have two classes, i.e., land and water. 
            - background # 0, background mask has to end with 'background_mask.xxx'. In the background mask file, masked areas have value 255, unmasked areas have value 0.   
            - burn # 1, the first class name
            - unburn # 2, the second class name

    #define the indicator of the background class whose class identity is unknown
    #the color of the background class is defined as the first color in 'my_colors'
    background_class_indicator: 0
```

**4.1 Each class has a color.** The length of ***my_colors*** should be equal to the length of ***my_classes***, which are equals to ***num_classes*** + 1, because the presence of the ***background*** class.  

**4.2 Background class.** The ***background*** class consists of the pixels that are not relevant, and usually are not used in the classification. For example, the land pixels in ocean classification. Land masks are needed to mask out the land area, which is defined here as the background class. 

**4.3 Class names should be exactly the same with SIP GUI**. For example, if under ***my_classes***, there are two classes, i.e., 'aaa' and 'bbb'. Then, in SIP GUI, when you draw ground truth, you have to define 'aaa_train', 'bbb_train', or optionally, 'aaa_val', 'bbb_val' for validation, and 'aaa_test', 'bbb_test' for testing. 


# 5. Generate samples for training classifiers

There are two types of samples, i.e., patch samples and image samples. 

* **patch samples** are ***small image patches***, e.g., 5-by-5, or 11-by-11 small images, which are used as input to determine the class label of the center pixels in image patchs. The use of an image patch centered at the referenced pixel rather than a single pixel enables the utilization of local spatial information when determining the label of the referenced pixel. Patch samples are usually used in classic convolutional neural network (CNN) models whose input is an image patch and output is the class memebership of the centered pixel. 

* **image samples** are ***big sub-images***, e.g., 512-by-512, which are used as input to determine the class label of all pixels in the sub-image. Image samples are usually used in fully convolutional neural network (FCN) whose input is a sub-image and output is a label map involving the labels of all pixels in the sub-image. 

```
data_params: 


    img_data_params: #image input to FCN models
        split_size: 145
        overlap: 5

    patch_data_params: #patch input to CNN models with fully connected layers
        patch_size: 1  # **

        save_extracted_patches: False #whether the extracted patch samples will be saved

        is_dataset_cuda: False # 'True' means all patches deployed on cuda; 'False' means cuda will be used batch-wise; 
        extract_patch_cuda: False # 'True' means performing F.unfold() on cuda()

        to_extract_patches_blockwise: True
        extract_patches_block_size_r: 1000
        extract_patches_block_size_c: 1000

        to_predict_patches_blockwise: True
        predict_patches_block_size_r: 1000
        predict_patches_block_size_c: 1000
 
```

**5.1 patch samples** are defined by ***patch_data_params***. If ***save_extracted_patches*** is True, all patch samples will be saved into hard disk, so that next time they will be automatically loaded. But, if you have limited hard disk space, you may want to set it to False. 

**5.2 use GPU.** If ***extract_patches_cuda*** is True, GPU will be used to extract patches. If ***is_dataset_cuda*** is True, all extracted patches will be deployed onto GPU. If you have limited GPU memeory or too many patches, you may want to set it to False, and then only one batch will be deployed onto GPU when training, instead of all batches.

**5.3 blcokwise computation.** If ***to_extract_patches_blockwise***, patch samples will be extracted in a block wise manner, meaning that when extracting patches, SIP will load and scan each block instead of the whole image. You may want to use this preference, if you have large image or limited memory. Similarly, if ***to_predict_patches_block_wise*** is True, SIP will load and predict each block instead of the whole image in case of large image or limited memory resource. 

**5.4 image samples** are defined by ***img_data_params***, where ***split_size*** defines the size of the sub-images and ***overlap*** defines the overlapping width between two adjacent sub-images.    

# 6. Training parameters

Here, it defines all the parameters related to classifier training.

```
train_params: 
    lr: 0.0001
    epoch: 50  
    batch_size_train: 2000 # batch_size when doing training
    batch_size_val: 20000 # batch_size when doing val and test
    val_inter: 1 # if 1, then validation and test are used in each epoch 
    to_test: False # is True, both validation and test data will be used to evaluate training
    prop_test: 1 # if 0.1, then 10% of all test samples are used in each epoch
    prop_train: 0.0001 # if 0.1, then 10% of all train samples are used in each epoch
    var: 0.05 # std of noise added to input when using images as input 
    net_type: knn
 
    train_mask_save_options:
        save_train_mask: False
        img_format: png
    
    input_img_save_options:
        save_input_img: False # if True, original images will be saved
        img_format: png
        is_rgb: False # whether want to display rgb image, if not, all channels will be saved separately
        rgb_bands: 0_0_0 # specify r, g, and b channels
        vmin: 0 # vmin of gray image, [vmin, vmax] is the range that the colormap covers
        vmax: 255 # vmax of gray image, [vmin, vmax] is the range that the colormap covers

    map_save_options:
        save_classification_map: True
        img_format: png
        copy_map_to_raw_data_dir: True
        save_map_over_epoch: True
        use_background_pixels: True


train_result_params:
    train_log_file: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/save/model/ss_res_batchSize2000_epoch50_lr0.0001_time20200613223826_train.log
    model_file: ss_res_batchSize2000_bestEpoch50_lr0.0001_time20200613225304_weights.pkl
    train_config_file: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/save/model/ss_res_batchSize2000_bestEpoch50_lr0.0001_time20200613225304__time20200613225305_train_config.yaml

``` 

**6.1 which classifier to use** is defined by ***net_type***. 

**6.2 some parameters are only relevant to neural network models.** ***epoch*** defines how many iterations of trying out all training samples. ***batch_size_train*** and ***batch_size_val*** are respectively the batch sizes of the training and validation operation. ***val_inter*** defines how many epochs have to be finished before performing one validation of the classifier. ***var*** defines the standard deviation of the noise added to the input data when using fully convolutional neural network (FCN) classifiers. 

**6.3 use a subset of training or test samples.** If you do not want to use all the training or test samples, you can use ***prop_train*** to specify the percentage of training samples randomly picked up in training, and ***prop_test*** for the percentage of random test samples to use. 

**6.4 save training mask.** If you want to save training masks you can so do by setting ***save_train_mask*** and ***img_format***. 

**6.5 save input images.** If you want to save the input images into some custormized format, you can use parameters under ***input_img_save_options***. 

**6.6 save classification maps of the training images.** If you want to predict all pixels in the training images using the trained model, and generate classification maps, you can set ***save_classification_map*** to be True. ***copy_map_to_raw_data_dir*** will copy the maps from ***dirs/save/train*** to ***raw_img_dir*** for easier access in SIP. ***save_map_over_epoch*** enables saving classification maps for all epochs during training FCN classifiers. It does not work for the other classifiers. ***use_background_pixels*** determines whether the background pixels will be predicted and shown in the classification map as meaningful classes.   


**6.7 train result parameters** specify the log file, model file and a copy of the config file. These three parameters are automatically setted after training by the software. But, you can manually set these parameters, e.g., the model file, to enable the loading of a parcicular model for testing or prediction. 

# 7. Test parameters

SIP allows to define all parameters related to classifier testing. These parameters are similar to the training parameters. Please see the training parameters for how to set the parameters here.

```
test_params:
    batch_size: 20000 # batch size for test
    
    test_mask_save_options:
        save_test_mask: False
        img_format: png
        
    input_img_save_options:
        save_input_img: False # if True, original images will be saved
        img_format: png
        is_rgb: False # whether want to display rgb image, if not, all channels will be saved separately
        rgb_bands: 0_0_0 # specify r, g, and b channels
        vmin: 0 # vmin of gray image, [vmin, vmax] is the range that the colormap covers
        vmax: 255 # vmax of gray image, [vmin, vmax] is the range that the colormap covers
    map_save_options:
        save_classification_map: True
        img_format: png
        copy_map_to_raw_data_dir: True
        use_background_pixels: False

test_result_params:
    test_log_file: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/save/model/ss_res_batchSize2000_epoch200_lr0.0001_time20200418170713_time20200418171406_test.log
    test_config_file: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/save/model/ss_res_batchSize2000_bestEpoch198_lr0.0001_time20200418171144_time20200418171406_test_config.yaml
 
```


# 8. Prediction parameters

SIP allows to define all parameters related to map prediction. These parameters are similar to the training parameters. Please see the training parameters for how to set the parameters here. 

```
predict_params:
    
    batch_size: 20000 # batch size for predict
    
    input_img_save_options:
        save_input_img: False # if True, original images will be saved
        img_format: png
        is_rgb: False # whether want to display rgb image, if not, all channels will be saved separately
        rgb_bands: 0_0_0 # specify r, g, and b channels
        vmin: 0 # vmin of gray image, [vmin, vmax] is the range that the colormap covers
        vmax: 255 # vmax of gray image, [vmin, vmax] is the range that the colormap covers

    map_save_options:
        load_hardlabel_and_generate_map: False #if True, no prediction, just load label and generate map
        save_classification_map: True
        img_format: png
        copy_map_to_raw_data_dir: True
        use_background_pixels: True

predict_result_params:
    predict_log_file: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/save/model/svm_batchSize2000_epoch50_lr0.0001_time20200613003824_time20200613141535_predict.log
    predict_config_file: /home/l44xu/SIP/data/landsat8_preprocessed_imgs/all_data/save/model/svm_time20200613020453_time20200613141535_predict_config.yaml

```

# 9. Define different classifiers

SIP offers three different types of classifiers, i.e., classic machine learning classifiers, patch-based convolutinoal neural network (CNN) classifiers, and the image-based fully covolutional neural network (FCN) classifiers. Please use  

* **classic classifiers** include the ***svm***, ***random forest (rf)*** and the ***k-nearest neighbors (knn)*** classifiers. These classifiers are in the ***classic_ml_model_list***. 

* **CNN classifiers** include spatial-spectral residual net ***ss-res*** and spatial-spectral CNN ***ss-cnn***. These classifiers are in the ***patch_model_list***. 

* **FCN classifiers** include deep image prior residual net ***dip_res*** classifier, which is in the ***dip_model_list***. 

```
net_params:
    
    ss_res: 
        channels: 20
        blocks: 3
        is_bn: True
        is_dropout: False
        p: -0.2
    
    ss_cnn: 
        feature_nums:
            - 16
            - 32
        is_bn: True
        is_dropout: False
        p: -0.2

    knn:
        innate: 
            n_jobs: -1
        grid_search:
            k_range:
                start: 1
                end: 30
            weight_options:
                - uniform
                - distance

    rf: 
        innate: 
            n_jobs: -1
            oob_score: True
            class_weight: balanced
        grid_search:
            n_estimators:
                - 10
                - 30
                - 50
                - 70
                - 100
                - 120
                - 150
            max_features:
                - auto
                - sqrt
                - log2
            #min_sample_split:
            #    - 2
            #    - 4
            #    - 6
            #min_sample_leaf:

    svm:
        innate:
            class_weight: balanced
            probability: True
        grid_search:
            kernel:
                - rbf
                - linear
            C:
                - 0.01
                - 0.1
                - 1 
                - 10
                - 100
            gamma:
                - 1
                - 0.1
                - 0.01
                - 0.001
            decision_function_shape: 
                - ovr
                - ovo
        crf:
            crf: unknown

    dip_res: 
        channels: 4
        blocks: 5
        act_fun: LeakyReLU  # LeakyRELU ReLU ...
        pad: reflection  # reflection zero ...
    
    

dip_model_list:
    - dip_res

patch_model_list:
    - ss_res
    - ss_cnn

classic_ml_model_list:
    - svm
    - rf
    - knn
``` 


