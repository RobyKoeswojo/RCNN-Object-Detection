# RCNN Object Detection

In this ``RCNN Object Detection`` notebook, RCNN is used to detect object. The object to be detected is raccoons.  
200 raccoon images and their corresponding annotations (ground truth bounding box) are used to train the RCNN.  

The steps to perform object detection with RCNN:
1. Create image dataset consisting of 2 classes; each of which contains images of raccoon and no raccoon
2. Train a classifier (CNN) to classify raccoon and no raccoon
3. Detect raccoon

The object to be detected here is raccoons. 

## Environment
The notebook is run with ``Tensorflow 2.6.0`` and ``OpenCV 4.1.2``

## Create dataset  
`Selective search` is used to create (propose) regions (an area) from an image.  
Each region is compared to the ground truth bounding box, specifically by calculating the intersection of union (IOU) between the region proposed to the ground truth bounding box of a raccoon. For regions with IOU > 0.7, they are collected to class ``raccoon``, whereas the regions with IOU < 0.05 are collected to class ``no raccoon``.  

| ![Dataset](https://github.com/RobyKoeswojo/RCNN-Object-Detection/blob/main/readmeImage/200proposedRegions.png?raw=true) |
|:--:| 
| The regions proposed by the `selective search` |

| ![Dataset](https://github.com/RobyKoeswojo/RCNN-Object-Detection/blob/main/readmeImage/raccoonDataset.JPG?raw=true) |
|:--:| 
| The `raccoon` and `no raccoon` dataset |

## Train classifier
A CNN is trained to classify 2 classes; raccoon and no raccoon.  
A pretrained `MobileNetV2` is used with all the weights frozen. On top of it, a bunch of dense layers are added.  
With only 5 epochs, the loss value for both train and validation set is already so low and the accuracy values are reaching 99%.

| ![Learning curve](https://github.com/RobyKoeswojo/RCNN-Object-Detection/blob/main/readmeImage/learningCurve.JPG?raw=true) |
|:--:| 
| Learning curve of the CNN classifier |

## Detection
The ``selective search`` algorithm is once again used to propose region from the image to be detected, creating region of interests (roi) which are fed as inputs to the classifier. Afterwards, the classifier classifies if the regions belong to ``raccoon`` or ``no raccoon``. For a region that is classified as raccoon, a bounding box is drawn. At the end, ``non maximum suppression`` algorithm selects one best bounding box out of many overlapping boxes drawn.  

| ![Detected object](https://github.com/RobyKoeswojo/RCNN-Object-Detection/blob/main/readmeImage/detection.JPG?raw=true) |
|:--:| 
| The detection: before (left) and after non maximum suppression(right) |

## Credits
The code and raccoon images used in this notebook are from the [pyimagesearch](https://www.pyimagesearch.com/2020/07/13/r-cnn-object-detection-with-keras-tensorflow-and-deep-learning/) tutorial.