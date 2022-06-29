# Face-Recognition-using-One-Shot-learning-technique-and-Siamese-Network

# 1. Import Dependencies
imported some library for run the code
* tensorflow
* cv2
* numpy 
* matplotlib 

# 2. Create Train and Test Data for Siamese Network
Created some folder for training and testing
*anchor
*positive
*negative

Download the data form
[cs.umass.edu/lfw](http://vis-www.cs.umass.edu/lfw/)

The images are different person and the image size is 250X250
 
In Image processing the image are converted into 100X100 and Scale image to be between 0 and 1

For training 70% data are user and the left 30% used in validation.


# 3. Model Engineering
In the modeling phase these processes are used.
#IMAGE

Model: "embedding"



| Layer (type)                |Output Shape              |Param #   |
|-----------------------------|:-------------------------|:--------:|
| input_image (InputLayer)    |[(None, 100, 100, 3)]     |0         |
|                             |                          |          |
| conv2d (Conv2D)             |(None, 91, 91, 64)        |19264     |
|                             |                          |          |
| max_pooling2d (MaxPooling2D)| (None, 46, 46, 64)       |0         |
|                             |                          |          |
|                             |                          |          |
| conv2d_1 (Conv2D)           |(None, 40, 40, 128)       |401536    |
|                             |                          |          |
| max_pooling2d_1 (MaxPooling | (None, 20, 20, 128)      |0         |
|2D)                          |                          |          |
|                             |                          |          |
| conv2d_2 (Conv2D)           |(None, 17, 17, 128)       |262272    |
|                             |                          |          |
| max_pooling2d_2 (MaxPooling | (None, 9, 9, 128)        |0         |
| 2D)                         |                          |          |
|                             |                          |          |
| conv2d_3 (Conv2D)           |(None, 6, 6, 256)         |524544    |
|                             |                          |          |
| flatten (Flatten)           |(None, 9216)              |0         |
|                             |                          |          |
| dense (Dense)               |(None, 4096)              |37752832  |
|                             |                          |          |
|=============================|==========================|==========|
|Total params: 38,960,448
|Trainable params: 38,960,448
|Non-trainable params: 0

Calculate The distance  |(img1)^2 - (img2)^2| of two images in L1Dist class


# 4. Make Siamese Model

Model: "SiameseNetwork" 
Here activation is sigmoid

|Layer (type)                 |  Output Shape          | Param #  |   Connected to                      |   
|-----------------------------|:-----------------------|:---------|-------------------------------------|
| input_img (InputLayer)      |   [(None, 100, 100, 3)]|  0       |    []                               |    
|                             |                        |          |                                     |    
|                             |                        |          |                                     |    
| validation_img (InputLayer) |   [(None, 100, 100, 3)]|  0       |    []                               |    
|                             |   )]                   |          |                                     |   
|                             |                        |          |                                     |  
| embedding (Functional)      |   (None, 4096)         |  38960448|    'input_img[0][0]',               | 
|                             |                        |          |     'validation_img[0][0]'          |
|                             |                        |          |                                     |
| distance (L1Dist)           |   (None, 4096)         |  0       |    'embedding[2][0]',               |
|                             |                        |          |     'embedding[3][0]'               |
|                             |                        |          |                                     |
| dense_1 (Dense)             |   (None, 1)            |  4097    |    ['distance[0][0]']               |
|                             |                        |          |                                     |
|=============================|========================|==========|=====================================|
|Total params: 38,964,545
||Trainable params: 38,964,545
|Non-trainable params: 0


# 4. Training
In training EPOCHS is 50
binary cross loss = 0.0001
* Record all of our operations
* Calculate Distance
* Check with data layer
* If the computer gets wrong then back propagates and gives the correct form.
* Calculate gradients
* Calculate updated weights and apply to siamese model
* Return loss


# 5. Testing 

In testing 
*system get two images and compare with the help of train model.
*Model give us a value 0 to 1
* We threshold the value to .5 to get correct accuracy.
* Compare the average result.
* save the result
