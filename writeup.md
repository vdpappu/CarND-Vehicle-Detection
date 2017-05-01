<h2>Vehicle Detection</h2>

[//]: # (Image References)

[image1]: ./images/00_HOG_Features.png
[image2]: ./images/01_HOG_Features.png
[image3]: ./images/02_Sliding_Windows.png
[image4]: ./images/03_Pipeline.png
[image5]: ./images/04_Pipeline.png


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
 ![alt text][image1] ![alt text][image2]
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

![alt text][image3]

<h4>Show some examples of test images to demonstrate how your pipeline is working. How did you optimize the performance of your classifier?</h4>

Following images shows the aggregated sliding window results(pipeline) of the test frames. 

![alt text][image4]![alt text][image5]


In order to improve the accuracy of classifier, following experments were done:
  * Various features were tried and experimented. Finally the HOG features on YUV color spaces are chosen for pipeline. Experiments can be found in 00_FeatureEngineering_SVMClassifier.ipynb and the final pipeline can be found in 01_SVMClassifier_Pipeline.ipynb
  * The hyper parameters for HOG feature extraction are experimented and tuned to work in the pipeline
  * SVM parameter tuning has no appearent impact on the model behavior

<h4>Provide a link to your final video output. Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)</h4>

The video can be found in following link. <<INSERT LINK HERE>>

<h4>Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.</h4>

* <b>False positives</b> are handled using the heatmap approach. The code for the approach is present in 01_SVMClassifier_Pipeline.ipynb cell-2. add_heat() and apply_threshold() methods are used for the same.
* The methods are written to find the accumulated detections with in a region and shed the random detections 
*<b>Overlapping bounding boxes</b> are aggregated using the draw_labeled_bboxes_vid() method that takes group of overlapping bounding boxes and draws aggregated bounding box that cover all boxes within

<h4>Briefly discuss any problems / issues you faced in your implementation of this project. Where will your pipeline likely fail? What could you do to make it more robust?</h4>

Despite the pipeline working well with acceptable false positives and near zero misses in detecting vehicles, I see following limitations:
  * The pipeline is not tested in varying climate conditions. The pipeline may not be effective if the visibility if low
  * A deep learning approach can make the pipeline more robust. The extracted features may not cover all the aspects of the vehicle. 
  * The sliding window approach is brute-force. A sophisticated approaches like edge boxes can help to scale the model
