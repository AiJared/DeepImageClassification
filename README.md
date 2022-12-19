# DeepImageClassification
A deep learning project for image classification, training and testing using tensorflow

This project is my first image classification project on deep learning using tensorflow framework and keras.
Here I am classifying five types of flowers namely; roses, dandelion, tulips, sunflowers and daisy


First, I import the necessary libraries as show in the code snippet below![Screenshot 2022-12-12 074228](https://user-images.githubusercontent.com/78556152/208366501-f7c4a21e-f1df-40ca-b3d6-eb3c4d668410.png)


Then download and explore the flowers images dataset

![Screenshot 2022-12-12 074510](https://user-images.githubusercontent.com/78556152/208367054-61d3cf33-2677-46a8-87d4-9d7a05c0d778.png)

Here are some images of roses

![Screenshot 2022-12-12 074609](https://user-images.githubusercontent.com/78556152/208367514-4f29785b-4dfc-4292-9681-a04c0aa243f4.png)


## Load Data
Use Keras Utility to load the dataset , i.e,  use the tf.keras.utils.image_dataset_from_directory utility

Before loading the data, first define some parameters for the loader.

Use validation split when developing your model with 80% of your images for training and 20% for validation![Screenshot 2022-12-12 074850](https://user-images.githubusercontent.com/78556152/208369339-5e07ddd2-89a8-4c53-ad2b-a3aeeff2abfe.png)


## Visualize the Data

Below are some of the images in my dataset

![Screenshot 2022-12-12 074946](https://user-images.githubusercontent.com/78556152/208369889-f0ed49dd-6d1d-4b2d-9753-9217d7d7837a.png)


## Configuring the Dataset for performance

I used buffere prefetching to yield data from the disk without having I/O become blocking. I used two methods when loading data

Dataset.cache - to keep the images in the memory after they are loaded off disk during the first epoch

Dataset.prefetch - to overlap data preprocessing and model execution while training
![Screenshot 2022-12-19 103155](https://user-images.githubusercontent.com/78556152/208371449-478aef66-8e2c-4651-a232-37720d86b643.png)

## Standardize the Data

I standardized RGB channel values from [0, 255] range to [0, 1] range by using tf.keras.layers.Rescalling

## A Basic Keras Model

The Keras Sequential model consists of three convolution blocks (tf.keras.layers.Conv2D) with a max pooling layer (tf.keras.layers.MaxPooling2D) in each of them. There's a fully-connected layer (tf.keras.layers.Dense) with 128 units on top of it that is activated by a ReLU activation function ('relu')

![Screenshot 2022-12-19 104013](https://user-images.githubusercontent.com/78556152/208372711-97cf074d-4d0e-40a9-806b-a82a66c5a81f.png)

## Compile the model

To compile my model I chose the tf.keras.optimizers.Adam optimizer and tf.keras.losses.SparseCategoricalCrossentropy loss function. To view training and validation accuracy for each training epoch, I passed the metrics argument to Model.compile.

![Screenshot 2022-12-19 104418](https://user-images.githubusercontent.com/78556152/208373548-be317984-e125-4cfb-ab04-abc2ebcbc6d7.png)

## Model Summary

Below are the layers of the network which are viewed by the keras Model.summary method
![Screenshot 2022-12-19 104854](https://user-images.githubusercontent.com/78556152/208374095-6c1ddb11-958a-4ef9-be6d-6862d04e1287.png)


## Train the Model
I trained the model for 10 epochs using the keras Model.fit method

![Screenshot 2022-12-19 111043](https://user-images.githubusercontent.com/78556152/208378069-024934aa-6720-4c31-8699-e9f7ca3e0a9e.png)

## Visualize Training Results

To visualize my training results, I created plots of loss and accuracy on training and validation sets

![Screenshot 2022-12-19 111556](https://user-images.githubusercontent.com/78556152/208378879-001b01ed-8383-4e9b-9b7e-f3d868e01360.png)

The plots show that training accuracy and validation accuracy are off by large margins, and the model has achieved only around 65% accuracy on the validation set.

## Overfitting

In the plots above, the training accuracy is increasing linearly over time, whereas validation accuracy stalls around 60% in the training process. Also, the difference in accuracy between training and validation accuracy is noticeable—a sign of overfitting.

When there are a small number of training examples, the model sometimes learns from noises or unwanted details from training examples—to an extent that it negatively impacts the performance of the model on new examples. This phenomenon is known as overfitting. It means that the model will have a difficult time generalizing on a new dataset.

There are multiple ways to fight overfitting in the training process. I am going to use data augmentation and add dropout to this model.

## Data Augmentation

Data augmentation takes the approach of generating additional training data from existing examples by augmenting them using random transformations that yield believable-looking images. This helps expose the model to more aspects of the data and generalize better.

I implemented data augmentation using the following Keras preprocessing layers: tf.keras.layers.RandomFlip, tf.keras.layers.RandomRotation, and tf.keras.layers.RandomZoom.


Visualize a few augmented examples by applying data augmentation to the same image several times


![Screenshot 2022-12-19 112437](https://user-images.githubusercontent.com/78556152/208380498-31e571b7-98a4-4f9c-bb0e-263ac7d4e089.png)

## Dropout

Another technique to reduce overfitting is to introduce dropout{:.external} regularization to the network.

When you apply dropout to a layer, it randomly drops out (by setting the activation to zero) a number of output units from the layer during the training process. Dropout takes a fractional number as its input value, in the form such as 0.1, 0.2, 0.4, etc. This means dropping out 10%, 20% or 40% of the output units randomly from the applied layer.

I created a new neural network with tf.keras.layers.Dropout

![Screenshot 2022-12-19 113027](https://user-images.githubusercontent.com/78556152/208381676-38d0f894-d505-431e-bdfa-bc1e888b0669.png)

I compiled and trained the model again. Below are the training results of our new model


![Screenshot 2022-12-19 113319](https://user-images.githubusercontent.com/78556152/208382222-591a29d9-b6b6-4c1e-8bb7-4fafa796a750.png)


## Predict on New Data

I used my model to classify an image that wasn't included in the training or validation sets.

Note: Data augmentation and dropout layers are inactive at inference time.

![Screenshot 2022-12-19 115003](https://user-images.githubusercontent.com/78556152/208385351-40ed6a13-caab-45c7-9764-4da2692eb377.png)


# Save the Model
Finally I saved my model and served it with tensorflow serving in docker during deployment

![Screenshot 2022-12-19 115625](https://user-images.githubusercontent.com/78556152/208386894-bcd8e6cb-ab79-4b38-9033-02355c6b8059.png)
