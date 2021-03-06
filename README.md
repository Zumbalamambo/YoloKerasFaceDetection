# Yolo Keras Face Detection

Implement Face and Emotion detection , and Age and Gender Classification. (Experimental)

![Detected](https://github.com/abars/YoloKerasFaceDetection/blob/master/pretrain/demo/demo.jpg "Detected")

(image from wider face dataset)

# Overview

## Functions

Face Detection (Keras , Darknet , Caffe)

Hand Detection (Darknet , Caffe)

Age Classification (Keras)

Gender Classification (Keras)

## Requirements

Keras2

Darknet

Caffe

Python 2.7

Perl

OpenCV

## Implement Environment

Mac Pro 2013

MacOS X 10.12

PlaidML

# Demo

## Pretrained Model

### Age and Gender Classification

<https://gist.github.com/GilLevi/c9e99062283c719c03de>

Download gender_net.caffemodel and gender_net.prototxt and age_net.caffemodel and age_net.prototxt.

Put in pretain folder.

### Face Detection

from <https://github.com/dannyblueliu/YOLO-version-2-Face-detection>

<http://www.abars.biz/keras/face.prototxt>

<http://www.abars.biz/keras/face.caffemodel>

Download face.prototxt and face.caffemodel.

Put in pretain folder.

### Emotion Detection

<https://gist.github.com/GilLevi/54aee1b8b0397721aa4b>

Download EmotiW_VGG_S.caffemodel and EmotiW_VGG_S.caffemodel.prototxt (rename from deploy.prototxt).

Put in pretain folder.

### Pretrained Model Demo

Here is a run using reference model .

`python agegender_yolo.py caffe`

## Experimentl Model

### Age Classification

loss: 0.3099 - acc: 0.8952 - val_loss: 1.1309 - val_acc: 0.6939

<http://www.abars.biz/keras/train_age_vgg16.hdf5>

<http://www.abars.biz/keras/agegender_age_vgg16.prototxt>

<http://www.abars.biz/keras/agegender_age_vgg16.caffemodel>

### Gender Classification

loss: 0.0691 - acc: 0.9713 - val_loss: 0.2905 - val_acc: 0.9206

<http://www.abars.biz/keras/train_gender_vgg16.hdf5>

<http://www.abars.biz/keras/agegender_gender_vgg16.prototxt>

<http://www.abars.biz/keras/agegender_gender_vgg16.caffemodel>

### Hand Detection

IOU : 0.8

<http://www.abars.biz/keras/vivahand_tinyyolov1_19000.weights>

<https://github.com/abars/YoloKerasFaceDetection/blob/master/vivahand_tinyyolov1.cfg>

### Experimental Model Demo

Here is a run using hdf5.

`python agegender_yolo.py keras`

Here is a run using caffemodel.

`python agegender_yolo.py converted`


# Install

## Modify Darknet

Download Darknet and put in the same folder.

https://github.com/pjreddie/darknet

Compile with <https://github.com/abars/YoloKerasFaceDetection/blob/master/darknet_custom/yolo.c> for custom classes and custom cfg.

`void train_yolo(char *cfgfile, char *weightfile,const char *train_images,const char *backup_directory)`

`draw_detections(im, l.side*l.side*l.n, thresh, boxes, probs, voc_names, alphabet, l.classes);`

`int class = find_int_arg(argc, argv, "-class", 20);`

`char *train_images = find_char_arg(argc, argv, "-train", 0);`

`char *backup_directory = find_char_arg(argc, argv, "-backup", 0);`

`else if(0==strcmp(argv[2], "train")) train_yolo(cfg, weights, train_images, backup_directory);`

`else if(0==strcmp(argv[2], "demo")) demo(cfg, weights, thresh, cam_index, filename, voc_names, class, frame_skip, prefix, out_filename);`

# Face Detection

## Create dataset

### widerface

Download wider face dataset (wider_face_split and WIDER_train folder) and put in the dataset/widerface folder.

http://mmlab.ie.cuhk.edu.hk/projects/WIDERFace/

Create dataset/widerface/WIDER_train/annotations_keras folder for keras.

`perl annotation_widerface_keras.pl`

Create datase/widerface/WIDER_train/annotations_darknet folder for darknet.

`perl annotation_widerface_darknet.pl`

### fddb

Download fddb dataset (FDDB-folds and originalPics folder) and put in the dataset/fddb folder.

http://vis-www.cs.umass.edu/fddb/

Create datase/fddb/FDDB-folds/annotations_darknet folder for darknet.

`perl annotation_fddb_darknet.pl`

## Train using Keras

### widerface

Download BasicYoloKeras and put in the same folder.

https://github.com/experiencor/basic-yolo-keras

Here is a train.

`cd basic-yolo-keras-master`

`python train.py -c ../cfg/widerface_keras.json`

## Train using Darknet

### widerface

Here is a train.

`cd darknet`

`./darknet yolo train ../cfg/widerface_tinyyolov1.cfg -train ../dataset/widerface/WIDER_train/annotations_darknet/train.txt -backup ./backup/ -class 1`

### fddb

`cd darknet`

`./darknet yolo train ../cfg/fddb_yolosmallv1.cfg -train ../dataset/fddb/FDDB-folds/annotations_darknet/train.txt -backup ./backup/ -class 1`

## Test using Darknet

### widerface

Here is a test.

`./darknet yolo test ../cfg/widerface_tinyyolov1.cfg ./backup/widerface_tinyyolov1_4000.weights ../dataset/widerface/WIDER_train/annotations_darknet/1.jpg -class 1`

Here is a run.

`./darknet yolo demo ../cfg/widerface_tinyyolov1.cfg ./backup/widerface_tinyyolov1_4000.weights -class 1`

### fddb

Here is a test.

`./darknet yolo test ../cfg/fddb_yolosmallv1.cfg ./backup/fddb_yolosmallv1_4000.weights ../dataset/fddb/originalPics/2002/07/19/big/img_18 -class 1`

Here is a run.

`./darknet yolo demo ../cfg/fddb_yolosmallv1.cfg ./backup/fddb_yolosmallv1_4000.weights -class 1`

## Convert to CaffeModel

Download pytorch-caffe-darknet-convert and put in the same folder.

https://github.com/marvis/pytorch-caffe-darknet-convert

Convert to Caffe model.

`cd pytorch-caffe-darknet-convert`

`python darknet2caffe.py ../cfg/widerface_tinyyolov1.cfg ./backup/widerface_tinyyolov1_200.weights widerface.prototxt widerface.caffemodel`

# Hand detection

## Create Dataset

Download viva hand dataset (detectiondata folder) and put in the datase/vivahand folder.

http://cvrr.ucsd.edu/vivachallenge/index.php/hands/hand-detection/

Create dataset/vivahand/detectiondata/train/pos annotations for darknet.

`perl annotation_vivahand_darknet.pl`

## Train using Darknet

Here is a train.

`cd darknet`

`./darknet yolo train ../cfg/vivahand_tinyyolov1.cfg -train ../dataset/vivahand/detectiondata/train/pos/train.txt -backup ./backup/ -class 4`

## Test using Darknet

Here is a test.

`./darknet yolo test ../cfg/vivahand_tinyyolov1.cfg ./backup/vivahand_tinyyolov1_4000.weights ../dataset/vivahand/detectiondata/train/pos/1_0000003_0_0_0_6.png -class 4`

Here is a run.

`./darknet yolo demo ../cfg/vivahand_tinyyolov1.cfg ./backup/vivahand_tinyyolov1_4000.weights -class 4`

# Age and Gender classification

## Create Dataset

Download AdienceBenchmarkOfUnfilteredFacesForGenderAndAgeClassification dataset (agegender folder)  and put in the dataset/agegender folder.

https://www.openu.ac.il/home/hassner/Adience/data.html#agegender

Create dataset/agegender/annotations for keras.

`perl annotation_agegender_keras.pl`

## Train using Keras

Run classifier task using keras.

`python agegender_train.py agegender`

`python agegender_train.py age`

`python agegender_train.py gender`

## Test using Keras

Test classifier task using keras.

`python agegender_predict.py agegender`

`python agegender_predict.py age`

`python agegender_predict.py gender`

Demo classifier task using keras and opencv.

`python agegender_recognize.py agegender`

`python agegender_recognize.py age`

`python agegender_recognize.py gender`

Demo classifier task using keras and yolo.

`python agegender_yolo.py keras`

# Emotion classification

## Create Dataset

Download FER2013 dataset.

https://www.kaggle.com/c/challenges-in-representation-learning-facial-expression-recognition-challenge/data

## Train

Implementing.

# Related Work

<https://www.openu.ac.il/home/hassner/projects/cnn_agegender/>

<https://github.com/dpressel/rude-carnie>

<https://how-old.net/>
