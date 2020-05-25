# Fast and Accurate Image Super Resolution by Deep CNN with Skip Connection and Network in Network

## Shristi Kumari (2016B4A70574P), Akriti Garg(2016B4A70480P)

[Project Presentation]()

## Overview

This is an implementation of "Fast and Accurate Image Super Resolution by Deep CNN with Skip Connection and Network in Network" a deep learning based Single-Image Super-Resolution (SISR) model. A highly efficient and faster Single Image Super-Resolution (SISR) model with Deep Convolutional neural networks (Deep CNN) is proposed in the paper which achieves state-of-the-art performance at 10 times lower computation cost. Single Image Super-Resolution (SISR) is used in many fields like security video surveillance and medical imaging, video playing, websites display.

The current Single Image Super Resolution models involve large computations and are not suitable for network edge devices like mobile, tablet and IoT devices.DCSCN model provides state-of-the-art performance with 10 times lower computation cost using parallelized 1x1 CNNs which not only reduces the dimensions of the previous layer for faster computation with less information loss, but also adds more nonlinearity to enhance the potential representation of the network

The DCSCN model consists of the following 2 parts : 

**1. Feature Extraction Network**

**2. Image Detail Reconstruction Network**

The Feature Extraction Network part consists of 7 sets of 3x3 CNN, bias and Parametric ReLU units. Each output of the units is passed to the next unit and simultaneously skipped to the reconstruction network. Parametric ReLu is used to solve “Dying ReLu “ problem as it prevents weights from learning a large negative bias term and leads to better performance. The Image reconstruction network consists of 2 parallelized CNN blocks which are concatenated together. The first consists of 1x1 convolution layer with PRelu and the second consists of a 1x1 layer followed by a 3x3 layer with PRelu as Activation function. After this  a 1x1 CNN layer is added. 1x1 CNNs are used to reduce the input dimension  before generating the high resolution  pixels.

The image below shows the DCSCN model structure.

<img src="">

## Requirements and Environment setup

python > 3.5

tensorflow > 1.0, scipy, numpy and pillow

## Data Description


## Data augmentation

To get a better performance, data augmentation is needed. You can use **augmentation.py** to build an augmented dataset. The arg, augment_level = 4, means it will add right-left, top-bottom and right-left and top-bottom fillped images to make 4 times bigger dataset. And there **yang91_4** directory will be generated as an augmented dataset.

To have better model, you should use larger training data like (BSD200 + Yang91) dataset.

```
# build 4x augmented dataset for yang91 dataset (will add flipped images)
python augmentation.py --dataset yang91 --augment_level 4

# build 8x augmented dataset for yang91 dataset (will add flipped and rotated images)
python augmentation.py --dataset yang91 --augment_level 8

# train with augmented data
python train.py --dataset yang91_4
```


## Sample result

<img src="https://raw.githubusercontent.com/jiny2001/dcscn-super-resolution/master/documents/result.png" width="864">


Our model, DCSCN is much lighter than other Deep Learning based SISR models. Here is a comparison chart of performance vs computation complexity from our paper.

<img src="https://raw.githubusercontent.com/jiny2001/dcscn-super-resolution/master/documents/compare.png" width="600">





## Result of PSNR


## Result of SSIM


## Instructions to Run

## Data Augmentation
Execute the following command on dataset of your own choice. Three data augmentation techniques have been implemented in aug.py

. Execute **evaluate.py** with these args below and you get results in **output** directory. When you want to evaluate with other parameters, try training first then evaluate with same parameters as training have done. Results will be logged at log.txt, please check.


```
# evaluating set14 dataset with normal model
python evaluate.py --test_dataset set14 --dataset yang_bsd_4 --filters_decay_gamma 1.5 --save_results True

# evaluating set5 dataset with compact model
python evaluate.py --test_dataset set5 --dataset yang_bsd_4 --save_results True --filters 32 --min_filters 8 --nin_filters 24 --nin_filters2 8

# evaluating all(set5,set14,bsd100) dataset with large model
python evaluate.py --test_dataset all --dataset yang_bsd_8 --layers 10 --filters 196 --min_filters 48 --last_cnn_size 3
```

## Apply to your own image

Place your image file in this project directory. And then run "sr.py --file 'your_image_file'" to apply Super Resolution. Results will be generated in **output** directory. Please note you should use same args which you used for training.

If you want to apply this model on your image001.png file, try those.

```
# apply super resoltion on image001.jpg (then see results at output directory)
python sr.py --file your_file.png --dataset yang_bsd_4 --filters_decay_gamma 1.5 

# apply super resoltion with compact model
python sr.py --file your_file.png --dataset yang_bsd_4 --filters 32 --min_filters 8 --nin_filters 24 --nin_filters2 8

# apply super resoltion with large model
python sr.py --file your_file.png --dataset yang_bsd_8 --layers 10 --filters 196 --min_filters 48 --last_cnn_size 3

# apply super resoltion with large model for scale x3
python sr.py --file your_file.png --dataset yang_bsd_8 --layers 10 --filters 196 --min_filters 48 --last_cnn_size 3 --scale 3
```

## How to train

You can train with any datasets. Put your image files as a training dataset into the directory under **data** directory, then specify with --dataset arg. There are some other hyper paramters to train, check [args.py](https://github.com/jiny2001/dcscn-super-resolution/blob/master/helper/args.py) to use other training parameters.

Each training and evaluation result will be added to **log.txt**.

```
# training with yang91 dataset
python train.py --dataset yang91

# training with larger filters and deeper layers
python train.py --dataset yang91 --filters 128 --layers 10

# after training has done, you can apply super resolution on your own image file. (put same args which you used on training)
python sr.py --file your_file.png --dataset yang91 --filters 128 --layers 10
```





## Visualization

During the training, tensorboard log is available. You can use "--save_weights True" to add histogram and stddev logging of each weights. Those are logged under **tf_log** directory.

<img src="https://raw.githubusercontent.com/jiny2001/dcscn-super-resolution/master/documents/model.png" width="400">

Also we log average PSNR of traing and testing, and then generate csv and plot files under **graphs** directory. Please note training PSNR contains dropout factor so it will be less than test PSNR. This graph is from training our compact version of DCSCN.

<img src="https://raw.githubusercontent.com/jiny2001/dcscn-super-resolution/master/documents/graph.png" width="400">
