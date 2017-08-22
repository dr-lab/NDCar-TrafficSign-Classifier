Traffic Sign Recognition
=

Build a Traffic Sign Recognition Project
-

The goals / steps of this project are the following:

* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report




[image1]: ./test-results/visulization.png "Visualization"
[image2]: ./test-results/top5.png "Top 5 Softmax"
[image3]: ./test-results/prediction.png "Prediction"
[image4]: ./test-results/post-augmentation-histogram.png "post augmentation histogram"
[image5]: ./test-results/y-class-histogram.png "Y class histogram"
[image6]: ./test-results/augmentaion-compare.png "Augmentation compare"
[image7]: ./test-results/LeNet.png "LeNet"


Data Set Summary & Exploration
-

I used the numpy library to calculate summary statistics of the traffic
signs data set:

Number of training examples = 34799
Number of testing examples = 12630
Image data shape = (32, 32, 3)
Number of classes = 43



Here is an exploratory visualization of the data set. It is a bar chart showing how the occurrences of each traffic sign .

![alt text][image5]

Design and Test a Model Architecture
-


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

![alt text][image7]


Bellow is the detail explanations:

        
0. **Input**         		| 32x32x3 RGB image   							
1. **Layer1**
 Convolution 3x3     	| 1x1 stride, VALID padding, outputs 28x28x6 	
 Max pooling	      	| 2x2 stride,  outputs 14x14x6				    
2. **Layer2**
 Convolution 3x3	    | 1x1 stride, VALID padding, outputs 10x10x16   
 Max pooling	      	| 2x2 stride,  outputs 5x5x6				    
3. **Layer3**
 Fully connected RELU  | output is 120     			  			    
4. **Layer4**
 Fully connected RELU  | output is 84     		    	  			    
5. **Layer5**
 Fully connected Softmax| output is 43									



To train the model, I used the **softmax_cross_entropy_with_logits**, and **Adam Optimizer** with following parameters:
        learning_rate = 0.001
        BATCH_SIZE = 128
        EPOCHS = 30


My **final model results** were:
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

        After use the validation data from the training data, I can get 98%+ validation accurancy.


 

Test a Model on New Images
-

I choose 10 german traffic signs downloaded from google. Following is the test results

![alt text][image3]


The model was able to correctly guess 7 out of 10 traffic signs, which gives an test accuracy of **70%**. 

**Image Qualities for testing**
Since in the model, we transform the images with contrast and sharpen, which may impact the accuracy of the test results, especially depends on the test images' color contrasting.
If the test image has good quality even if after sharpened and contrasted, then the test accuracy will be better.



**Over-fitting or Under-fitting Discussion**
Comparing the test data set got from the file which accuracy is 81.6%, I think the reason that the real test images have low accuracy is because
of the speed limit traffic signs are not been classified correctly. Differences of "Speed limit signs" are only the digital numbers within the big circle, 
and my classifier does not focus on the digital number, hence the accuracy of the 
Speed Limit is NOT good in my classifier. In my test cases, speed limit signs are "70" and "100", which has one digit of shared "0".
The image transform during the pre-processing should contribute the "Speed Limit Sign" classification since we use Sharpen and Constrasting which do remove some of the unique features of each sign.
\


With the **top 5 softmax probabilities**, we can see how the model do the guessing. 

![alt text][image2]


Optional Visualization
-

I also did some trying in the optional question, and I can visualize the conv1 and conv2 layers data, bellow is one sample of one traffic sign:

![alt text][image1]

