---
title: "CIFAR-10 Image Classification Using ResNet"
excerpt: "Harnessed Convolutional Neural Network (CNN) layers to build the ResNet Model which is trained on a set of 50000 images with 10 labels and classified a set of 10000 images into the same 10
labels with 82% accuracy"
collection: portfolio
---


## GitHub Repository:
[Repository](https://github.com/sameerprasadkoppolu/CIFAR-10-Image-Classification-Group-10?tab=readme-ov-file)


## Introduction:
Image Classification is the categorization of images into one of several classes or categories based on the object present in the images. It is implemented using Supervised Machine Learning models where the models are trained on a set of labeled images (training set) and then are used to predict the labels of the images that were not used in the training process but whose labels are known (test set). The predictions on this test set are then used to evaluate the model's performance.  
  
In this context, we have implemented 4 Supervised Machine Learning Models namely, the Random Forest Classifier, the Support Vector Machine with Kernel, the Softmax Classifier, and the ResNet for image classification on the CIFAR-10 dataset, and their respective performances are discussed.


## Data Description:
Collected by Alex Krizhevsky, Vinod Nair, and Geoffrey Hinton, the CIFAR-10 dataset is a subset of the '80 million tiny images' dataset. It consists of small 32x32 images with 3 channels (RGB channels), where each image is labeled with one of ten classes. The classes are 'Airplane', 'Automobile', 'Bird', 'Cat', 'Deer', 'Dog', 'Frog', 'Horse', 'Ship', and 'Truck'. Each of these class values is represented as a number from 0 to 9 for each image where 0 corresponds to 'Airplane', 1 corresponds to 'Automobile', and so on and so forth. All of the classes are mutually exclusive in order to avoid confusion. This means that any image that is not a large truck is part of the 'Automobile' class and not the 'Truck' class. The 'Truck' class only has images of large trucks. Additionally, the number of images in each class is equal. This holds true in both the training set and the test set  
  
Each pixel in each image has a value between 0 to 255. This value indicates the color intensity with respect to the channel that the pixel is present in (i.e. color intensity of Red, Green, or Blue). These pixel values become the input features to the machine learning models in order to classify the image. When the data is loaded, it is split into a Training Set of 50000 images and a Testing Set of 10000 images. The data in the training and test sets are represented as 3 Dimensional Arrays. As mentioned above, each image has a height and width of 32x32 with 3 channels which are the RGB channels. This indicates that each image is a 3-dimensional tensor with 32 rows, 32 columns, and 3 color channels.  


## Data Preprocessing:
A common step prior to training any of the models is to normalize the pixel values for each image in the Training Set and in the Test Set. To perform normalization, we simply divide each pixel value by 255. The reasons for this are as follows:
* The model performance improves as a result of normalization due to the decrease in scale and distribution differences of pixel values across the images.
* Normalization helps to prevent numerical instability which further helps in avoiding the vanishing gradient and exploding gradient problem.
* Additionally, Normalization ensures that convergence is achieved faster during training since the range of pixel values is between 0 to 1.  

One additional step that is performed for the Random Forest Classifier, Support Vector Machine (SVM) with Kernel, and in the Softmax Classifier, is the flattening of the Training and Test datasets. Here each image gets converted to 2 dimensions with 3072 features (since 32*32*3 = 3072). Although flattening takes place in ResNet, it occurs much later in the neural network when we flatten the output feature map obtained from convolution that is to be used in the fully connected layers.  
  
Additionally, we perform PCA in SVM with Kernel by reducing the number of features from 3072 to 200. This was done due to the fact that applying the Radial Basis Function (RBF) kernel on 3072 features turned out to be computationally expensive.


## Modelling Results:
* **SoftMax Classifier:**  
  ![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/beb0982d-2c6b-4868-80cf-662b9b4325dd)

* **Random Forest Classifier:**  
  ![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/6c59d23d-f792-49f9-ae1b-6ea3c9694130)

* **Support Vector Machine (SVM) with Radial Basis Function (RBF) Kernel:**  
  ![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/13ffc69a-d2b4-4311-babb-3577759c4c38)

* **ResNet (CNN):**
  ![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/65c26b22-9327-4c31-8207-c6e93a21c2c3)

ResNet outperforms all the other models with an accuracy of 82%. SVM comes in second with an accuracy of 57.13%, followed by Softmax with 48.86% and Random Forest with 47.52%. (Note, all of these accuracies are based on the predictions made on the test set).  
  
In terms of computational complexity and memory usage, ResNet is a relatively complex model compared to the other three models, and it requires significant computational resources to train and run. On the other hand, SVM and Random Forest are relatively simpler models that require less computational resources and memory.  
  
In terms of training time, ResNet is a time-consuming model to train compared to the other models, and it may require specialized hardware such as GPUs to speed up the training process. On the other hand, SVM and Random Forest are relatively faster to train, and they can be trained on standard hardware.  
  
Regarding interpretability, SVM and Random Forest are more interpretable than ResNet since they are based on decision boundaries and decision trees, respectively, which are easier to visualize and understand. Softmax is also relatively interpretable since it is a linear model with a single layer.  
  
A major reason why ResNet displays the best performance among all of the other models is due to the fact it takes into account the benefits of Convolution that the other models ignore. Through convolution, the network is able to extract local characteristics in a hierarchical fashion from the input image. This is crucial for image classification since objects and features of interest might exist anywhere in a picture, and a convolutional layer can learn to recognize these features no matter where they are in the image.
