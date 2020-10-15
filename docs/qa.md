# Frequently asked questions.

## Q1: how do I make the background_mask file?

**A1:** First, the background_mask file should have a file name like  ***xxx_background_mask.tiff***, where ***xxx*** is your scene ID string. Second, pixels in masked areas, e.g., the land pixels in ocean classifications, have to have intensity values of 255 in a 8-bits image. And, pixels of unmasked areas have value of 0. 

## Q2: How do I set the class labels in SIP GUI before drawing ground truths?

**A2:** The class label has two parts, looks like ***xxx_###***, where ***xxx*** is the class name, and ***###*** is one of ***train***, ***val***, ***test***. The class name ***xxx*** has to be consistent with the class names under ***my_classes*** in the [config file](config_file.md). So, if you have a class label like ***ice_train***, then you will use it to draw the training samples for the ***ice*** class. 

## Q3: If I have 20 image scenes, how do I split them into training and test set?

**A3:** The simplest approach is use half for training and the rest half for testing. In order to do this, you only draw ***train*** and ***val*** ground truth samples for the 10 training scenes. And, for the rest of the 10 test scenes, you only draw ***test*** ground truth samples. If not, say you draw ***test*** samples on a training scene, then this scene will also be used to test the algorithmbased on these test samples. 

## Q4: Can I mosaic the classificaiton maps of different scenes into a single map that covers my study area?

**A4:** Yes, if your input data is geocoded, and you set ***is_geotiff*** to be True, and ***to_geocode_classification_map*** to be True in the [config file](config_file.md)



