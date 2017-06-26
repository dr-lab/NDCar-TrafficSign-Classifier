#**Traffic Sign Recognition** 


**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./test-results/visulization.png "Visualization"
[image2]: ./test-results/top5.png "Top 5 Softmax"
[image3]: ./test-results/prediction.png "Prediction"
[image4]: ./test-results/post-augmentation-histogram.png "post augmentation histogram"
[image5]: ./test-results/y-class-histogram.png "Y class histogram"
[image6]: ./test-results/augmentaion-compare.png "Augmentation compare"
[image7]: ./test-results/LeNet.png "LeNet"

###Data Set Summary & Exploration


I used the numpy library to calculate summary statistics of the traffic
signs data set:

Number of training examples = 34799
Number of testing examples = 12630
Image data shape = (32, 32, 3)
Number of classes = 43



Here is an exploratory visualization of the data set. It is a bar chart showing how the occurrences of each traffic sign .

![alt text][image5]

###Design and Test a Model Architecture


As a first step, I decided to convert the images to grayscale, but in my case I did not see better results, I skip the grayscaling. But I will some image manipulation in the data mock(augmentation) step.

After investigated the traffic signs distribution, I see that the training data is not evenly distributed. Some of the traffic signs has lot of samples while some have very few (~200) images only.

To add more data to the the data set, I use the technologies of image rotation, sharpen and increase constrasting.

Here is an example of an original image and an augmented image:

![alt text][image6]

The difference between the original data set and the augmented data set is the following:
1. the transformed image are processed by sharpening and increasing the contrasting
2. The Augmented images are been rotated in a randomly angels

During the augmentation, I check the total number of occurrences of each traffic sign, augmented more of the sign has few occurrences. This technology seems solve the issue I met at the beginning of overfitting.
Finally the augmented traffic sign training data histogram char is as bellow:

![alt text][image4]



My final model consisted of the following layers:

Briefly I use LeNet to train the data, there are totally 5 layers of the network. Bellow is the overall architecture I used:
![text] [image7]


Bellow is the detail explanations:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 

| Convolution 3x3     	| 1x1 stride, VALID padding, outputs 28x28x6 	|
| Max pooling	      	| 2x2 stride,  outputs 14x14x6				    |

| Convolution 3x3	    | 1x1 stride, VALID padding, outputs 10x10x16   |
| Max pooling	      	| 2x2 stride,  outputs 5x5x6				    |

| Fully connected RELU  | output is 120     			  			    |

| Fully connected RELU  | output is 84     		    	  			    |

| Fully connected Softmax| output is 43									|
|						|												|
|						|												|
 


To train the model, I used an softmax_cross_entropy_with)logits, and Adam Optimizer with following marameters:
learning_rate = 0.001
BATCH_SIZE = 128
EPOCHS = 30


My final model results were:
* training set accuracy of 98.1%
* validation set accuracy of 93.8%
* test set accuracy of 81.6%

If an iterative approach was chosen:
* What was the first architecture that was tried and why was it chosen?
First I use the validation data from file, but I only can get about 89% validation accurrency. Then I change the strategy, to load 10% of the data from the training data as validation data.

When augmenting the training data, I first compare the sample distribution, and mock up more data if the training traffic signs has less occurrences in the training dataset. This one do help remove the over fitting.


* What were some problems with the initial architecture?
Validation data should be similar as training data, otherwise the validation results will be low, and will not help judge the training quality.

* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.
* Which parameters were tuned? How were they adjusted and why?
After use the validation data from the training data, I can get 98%+ validation accurancy.


 

###Test a Model on New Images

I choose 10 german traffic signs downloaded from google. Following is the test results
![test] [image3]


The model was able to correctly guess 7 out of 10 traffic signs, which gives an accuracy of 70%. 



####3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

With the top 5 softmax probilities, we can see how the model do the guessing. 
![text] [image2]


I also did some trying in the optional question, and I can visualize the conv1 and conv2 layers data, bellow is one sample of one traffic sign:
![text] [image1]
