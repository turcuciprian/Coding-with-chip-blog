---
title: "Image Dataset Multiplier/Processing v1"
layout: post
date: 2023-06-03
---
When working with machine learning and image data, there are a lot of things you can do to make your model better. Cleaning Data is one of the first and most comonly used approaches necessary in training your model, but sometimes, you might also need to improve your model accuracy, by distorting the data.

With that in mind, I made a script that takes a folder with images and does the following modifications to them adding: blur, noise, darkens, ove-rexposes them while putting each modified version in it's coresponding folder name and at the end, copies all of them to a single folder and ads a prefix of each modification style to the file names.

On top of everything, it takes from a generated folder `masks` all the images and creates mask images for each modified picture, with each coresponding generated (with prefix) name

### [Repository Link](https://github.com/turcuciprian/BS_DPI)
---
# Usage Instructions

- install poetry on your system
- install python if not installed

### Install packages

- in your root folder switch to poetry environment:

`poetry shell`

- Install dependencies

`poetry install`

### Code instructions

- All the scripts are in separate py files in the scripts folder
- All you need to do is run `python all.py` inside of the scripts folder
  - create all the output folders and main root `before` and `after` folders
    #### - `!!! make sure you have images in the before folder`
    The script will
      - take all the images from the before folder and resize them and put them in the `resize` folder
      - greyscale all the images in the resize folder and save them in the `greyscale` folder
      - blur all the images in the resize folder and save them in the `blur` folder
      - over expose all the images in the resize folder and save them in the `over_expose` folder
      - darken all the images in the resize folder and save them in the `darken` folder
      - add noise to all the images in the resize folder and save them in the `noise` folder
    At the end, it will:
      - take all the images from each folder and put them in `after/all/images` with the prefix of each file being the parent pfoder name (ex: parentFolder_filename.jpg)
      - take all the images in the `after/mask` folder and use the above prefix name to generate that exact (with prefix) name, like the previous point, into the `after/all/masks` folder, so that all the `after/all/images` have a coresponding mask, with the image exact name (the version with the prefix)

      Basically it will generate an `after` folder with each image transformation as a folder name inside it, place all the images transformed after they are taken from the before folder and processed, and creates an after/all folder with `images` and `masks` folders inside of them, where all the images generated are combined with their parent folder name as a file prefix while it generates the masks for each image accourdingly.

      ATTENTION!
      1. The masks are images that need to exist with the same names exactly as the images in the `before` folder.
      2. For this version of the script, you need to run the all.py script twice. First time to generate the modified images and the second time to generate the masks for the modified images - this is because the after/mask folder does not exist and is generated (if you create it manually and place the masks in the re beforehand, it should work and require you to run python `all.py` only once)
      3. When you run `python all.py` you need to be inside the scripts folder.
      4. You can modify the parameters of how much blur, darkness,over-exposure, or noise is applied to the images in the all.py script, for each script function that is ran 

      Basically, that is all, I know I will be using this script for generating 
