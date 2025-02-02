# **Traffic Sign Recognition** 

## Project Report
---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./report_images/data_summery2.png "Visualization"
[image2]: ./report_images/gray_scale.png "Grayscaling"
[image3]: ./report_images/normalization.png "Normalization"
[image4]: ./report_images/test_data.png "test data"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is (27839, 32,32,3)
* The size of the validation set is (6960, 32, 32, 3) 
* The size of test set is (12630, 32, 32, 3)
* The shape of a traffic sign image is (32, 32, 3)
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how the data ...

![alt text][image1]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

As a first step, I decided to convert the images to grayscale because color will not affect how we detect pattern and gradient.

Here is an example of a traffic sign image before and after grayscaling.

![alt text][image2]

As a last step, I normalized the image data because this step will shape the data into uniform distribution.

Here is an example of an original image before and after normalization:

![alt text][image3]


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x1 gray scale image   							| 
| Convolution 5x5x1x6     	| 1x1x1x1 stride, VALID padding, outputs 28x28x6 	|
| RELU					|												|
| Max pooling	 1x2x2x1     	| 1x2x2x1 stride,  outputs 14x14x6 				|
| Convolution 5x5x6x16	    | 1x1x1x1 stride, VALID padding, outputs 10x10x16 	|
| RELU					|												|
| Max pooling	 1x2x2x1     	| 1x2x2x1 stride,  outputs 5x5x16 				|
| Flatten |outputs 400|
| Fully connected		|   outputs 120  |
| RELU					|												|
| Fully connected		|   outputs 84  |
| RELU					|												|
| Fully connected		|   outputs 43  |

 


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I finally adopted the hyperparameters as followings:
* Optimizer = Adam
* Epoch number = 30 
* Batch size = 100
* learnning rate = 0.0005

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 0.998
* validation set accuracy of 0.979
* test set accuracy of 0.890

If an iterative approach was chosen:
* What was the first architecture that was tried and why was it chosen?
* What were some problems with the initial architecture?
* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.
* Which parameters were tuned? How were they adjusted and why?
* What are some of the important design choices and why were they chosen? For example, why might a convolution layer work well with this problem? How might a dropout layer help with creating a successful model?

If a well known architecture was chosen:
* What architecture was chosen?
* Why did you believe it would be relevant to the traffic sign application?
* How does the final model's accuracy on the training, validation and test set provide evidence that the model is working well?

I choose the LeNet-5 architecture as my training model. 

The reason why I believe it is relevant to the traffic sign application is that the traffic signs are usually have specific and dominant features and patterns with relative low complexity. Different from most other CNNs which always sacrifice more resources and complicated network model development while pursuing higher performance, LeNet-5 could achieve the same purpose with a lower complexity. 

Here I will brief discuss how I find the best solution by tuning hyper parameters:
First, I set `EPOCHS = 30`, `BATCH_SIZE = 128`, `RATE = 0.001`. With training process moving forward, the accuracy is keep increasing, but the best performance is just arround 85%. Hence, for next step, I decided to adjust `EPOCHS = 60`, but just found that the best performance barely increased 2%. This made me think of that the learning rate might be too large, so I adjusted the learning rate to be `RATE = 0.0005` and the performance increased dramatically to 94%. At last, I adjusted the `BATCH_SIZE = 100`, with the assumption that the smaller the batch size is, the more training iterations in one training epoch and it is similar to adding more stochasticity into the optimizer during its gradient descent. And this drived me to the final optimal performance.

There are several aspects to justify the validity of the model:
* The accuracies of training set and validation set are pretty close to each other
* The error of accuracy betwen training set and test set is less than 0.1


### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image4] 

The first image might be difficult to classify because the traffic sign color is similar to its background and when we subsample the image size into `32x32x3`, the boundary of this borad is mostly blurred. Also there are chances that if the light in the image is too dark or too bright, it will also increase the difficulty of detection out of the similar reason. Another case (not listed in the selected images) is that the traffic sign is partly covered by obstacles like trees. This will change the pattern of the traffic sign and also might lead to misdetection.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Turn left ahead     		| Turn left ahead   									| 
| Priority Road     			| Priority Road										|
| Speed limit(30km/h)					| Speed limit(30km/h)							|
| right of way at the next intersection      		|  right of way at the next intersection 				|
| Go straight or right			| Go straight or right						|


The model was able to correctly guess 5 of the 5 traffic signs, which gives an accuracy of 100%. This compares favorably to the accuracy on the test set of 89%

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 21th cell of the Ipython notebook.

Taking the second image as an example, the model is relatively sure that this is a Turn left ahead (probability of 0.99), and the image does contain a stop sign. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 0.99        			| Turn left ahead  									| 
| 2e-07   				| No passing										|
| 1.7e-07					| No vehicles										|
| 4e-10	      			| Speed limit (100km/h)				 				|
| 2e-12				    | Slippery Road      							|

### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?


