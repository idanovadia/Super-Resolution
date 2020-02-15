# Super-Resolution
Using ML algorithms to do Super Resolution 

Stage 1:
Create dataset – we would like to create a dataset for the training of super-resolution network
Our basic dataset for this task will be the PascalVOC 2007 dataset which I shared with you here:

https://drive.google.com/open?id=1UFoUNOznWKg9lOgYtuawgSasd5_rLWDF 

Your first task was to create a dataset with images of 3 different sizes:

X - 72x72x3 ; y_mid – 144x144x3 ; y_large – 288x288x3

Once you have loaded and generated the above arrays of input images, we would like to split them into training and validating our model, for simplicity, we will use the first 1000 images for validation and the rest for training. (note you should have 5011 images in total but are welcome to add more if you wish to)
* reminder - in class we only took 20 images to make the process of loading and processing faster. This is a good practice for quick development of the loading pipeline. Once everything is working the way we expect it to work, we can increase the number of images we load and be sure that the process runs smoothly.
Next, I asked you to present few images so that we can compare the training with our desired labels, this process is good for verifying that we have got the input we want and will also be useful for visual assessment of our model’s results so make sure you write it as a function for later reuse.
You should get something like this as an output:

![](https://github.com/idanovadia/Predict-Future-Sales---Kaggle-Competition/blob/master/photos/3.png)

Step 2:
Create an initial model
In this step we created the following (vanilla) model:

![](https://github.com/idanovadia/Predict-Future-Sales---Kaggle-Competition/blob/master/photos/3.png)


We fitted this model to our data using X_train and y_mid_train only
Step 3:
We added another block to our model so now we have both 144x144x3 output along with 288x288x3 output as follows:
(make sure you understand the dimensions in each step of the process)

![](https://github.com/idanovadia/Predict-Future-Sales---Kaggle-Competition/blob/master/photos/3.png)

Step 4:
Add residual blocks into the process
The residual blocks that we added were defined as a function with no arguments that returns a model.
We later used this residual block definition within our model prior to every UpSampling layer

![](https://github.com/idanovadia/Predict-Future-Sales---Kaggle-Competition/blob/master/photos/3.png)

Step 5:
replace the residual blocks we defined above with a dilated convolutional block as described below:

![](https://github.com/idanovadia/Predict-Future-Sales---Kaggle-Competition/blob/master/photos/3.png)

Step 6:
Add pretrained network (efficientnet/VGG16/resnet) feature extractor to the network (note that the input to the network is only being read once)

![](https://github.com/idanovadia/Predict-Future-Sales---Kaggle-Competition/blob/master/photos/3.png)

This step will be a part of what we’ll do next class:
Step 7: replace the Upsampling2D layer with a lambda layer that receives scaling factor as input and returns tf.depth_to_space(x,scale)
* Note that the number of filters in the preceding layer (num of input channels to the depth_to_space layer) should be divisible by (scale^2)

![](https://github.com/idanovadia/Predict-Future-Sales---Kaggle-Competition/blob/master/photos/3.png)

