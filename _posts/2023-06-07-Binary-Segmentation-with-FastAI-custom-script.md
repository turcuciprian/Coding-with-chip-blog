---
title: "Binary Segmentation with FastAI - custom script"
layout: post
date: 2023-06-07
---

Create masks for any images, in any pictures, and with a dataset as low as 10 pictures and masks to train from, you can get great results.

I have used the FastAI library and created a script that can train a machine learning model with, again, as little as 10 pictures with masks and the result of that model when testing it is absolutely incredible considering the training dataset size.

### [ - DEMO - ](https://youtu.be/Qni5JU8c7JQ)

The script is avaolable on my repository here:

### [Repository Link](https://github.com/turcuciprian/binary_segmentation)

---

# Prerequisites Instructions

- install poetry on your system
- install python if not installed
- Images in a data/images folder
- Masks in a data/masks folder
- the masks need to be png files
- the masks need to have the same file names as the images
- the images can be jpg files
- the images and the masks should have the exact width and height
- the width and height should be around max 800x600 (for video card memory limit training purposes)

### Install packages

- in your root folder switch to poetry environment:

`poetry shell`

- Install dependencies

`poetry install`

### Train

after activating the poetry environment and installing poetry once (making sure you have all the required dependencies installed), just run:

``python train.py`

When running it, make sure you are in the root directory of the repository

### Inference (test/use the model)

After training is done, the model will be available in the models folder. It will have the name you have it inside the train.py at the end when the model is saved.
The script uses a `./test/input/` folder and takes all the images from that folder. So whatever you want to run your model against as images, put them in that folder.

To start inference, run the following command in the root of the repository:

``python inferrence.py`

All of the predicted masks will be overlayed over the original images and given an opacity. Then they will be saved as new images in the folder `./test/input/`

When running it, make sure you are in the root directory of the repository
