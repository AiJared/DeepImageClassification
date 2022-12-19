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



