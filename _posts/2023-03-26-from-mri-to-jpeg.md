---
layout: page
title: "Dicom MRI to Jpeg file in python"
permalink: /posts/testx.html
---

# Dicom MRI to Jpeg file in python

In this post I'm going to show you how I read DICOM images in python and saved them as jpg files

Here's the code:

```
from medpy.io import load
import matplotlib.pyplot as plt
from os import listdir

folder_path = 'DICOM/PA1/ST1'
for folder in listdir(folder_path):
    for image in listdir(folder_path+'/'+folder):
        print(image)
        image_data1= load(folder_path+'/'+folder+'/'+image)
        img = image_data1[0] 
        plt.imshow(img)
        plt.savefig('images_exports/'+folder+image+'.jpg')
```
