<h2>Vehicle Detection</h2>

The goals / steps of this project are the following:

* Use HOG features to build a vehicle detection algorithm using SVM
* Implement a sliding-window technique and use the trained classifier to search for vehicles in images
* Run the pipeline on a video stream and create a heat map of recurring detections frame by frame to reject outliers 
  and follow detected vehicles.
* Estimate a bounding box for vehicles detected

<h3>Rubric Points</h3>

<h4>Explain how (and identify where in your code) you extracted HOG features from the training images. 
Explain how you settled on your final choice of HOG parameters.</h4>

The code for HOG feature exploration can be found in 00_FeatureEngineering_SVMClassifier.ipynb cell-2. Following steps are followed

* Read the vehicle and non- vehicle sample images
* Convert image to grey scale to extract hog features
* Use hog() method to get the hog features and to get the corresponding hog image. Following parameters were tuned and experimented with:
  * orientations
  * pixels_per_cell
  * cells_per_block
 * Sample HOG features are shown below
 <<<INSERT IMAGE HERE>>>
Up on experimentation, hog features across YUV channels with orientations = 8, pixels_per_cell = 8 and cells_per_block = 2 gave 
good accuracy with SVM classifier built for pipeline

<h4>Describe how (and identify where in your code) you trained a classifier using your selected HOG features 
(and color features if you used them). </h4>

The code for classifier can be found in 00_FeatureEngineering_SVMClassifier.ipynb cell-3-6
  * SVM classifier has been tested and used to classify vehicles and non-vehicles
  * Initially, following features were extracted and used as features for classifier:
    * Spatial features - The images are resized to 16X16 and the spatial features are extracted
    * Histogram features
    * HOG features
   * Above features are extracted and tested with various color spaces - RGB, HSV, LUV, HLS, YUV, YCrCb
   * The features are scaled and normalized to ensure consistency across features
   * The data provided was shuffled and random split of 80-20 is chosen for train and test
   * An SVM classifier was trained with linear kernel and c=0.1 
   * Accuracy on test set with above features and model hyper-parameters - 99.2%

<h4>Describe how (and identify where in your code) you implemented a sliding window search. How did you decide what scales to search and how much to overlap windows? </h4>

In order to be able to find the vehicles in image/video frame, a sliding window approach has been used. Code for sliding window approach with varying search window sizes can be found in 01_SVMClassifier_Pipeline.ipynb cell-2
* An approximate size of the car is calculated across various positions in the image (along y-axis) and depending on the position of the sliding window search, corresponding window size was used
* Several overlap values are experimented and value of 0.85 was finally chosen
* Sample image of sliding window 


