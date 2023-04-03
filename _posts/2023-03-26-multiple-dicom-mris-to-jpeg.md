---
title: "Multiple Dicom MRIs to jpeg"
categories: machine-learning
layout: post
date: 2023-03-26

---

Here's the code for handling multiple MRI folders with DICOM mri images and how to save them into jpegs in organized folders

```
from genericpath import isfile
from medpy.io import load
import matplotlib.pyplot as plt
import os
def final_dir(path, start_level=1, level_skip=2):
    childfolders = os.listdir(path)
    if len(childfolders)==0:
        return path, childfolders
    new_path = path+'/'+childfolders[0]
    start_level=start_level+1
    if(len(childfolders)<level_skip):
        return final_dir(new_path, start_level)
    elif(start_level>level_skip):
        return path, childfolders
    else:
        return final_dir(new_path, start_level)

# (path and child folders to write to, base destination path)
def create_folders( final_dir_data,exports_path = './images_exports',):
    # split to tupils PATH & LIST OF CHILD FOLDERS
    path, child_folders = final_dir_data
    #Cycle trough child folders
    for folder in child_folders:
        # create path for child folder to write
        folder_path = exports_path+'/'+folder

        if not os.path.exists(folder_path):
            os.mkdir(folder_path)
    return path, child_folders





base_path = './MRI_images'
export_base_path = './images_exports'

# get first level Folders
first_level_path, first_level_folders = final_dir(base_path,1,1)
# create first level export folders //// with no returns
_, _ = create_folders((first_level_path,first_level_folders))
#--- get directories for final level paths
# cycle trough each folder
for folder in first_level_folders:
    # get the final level path and folders
    read_base_folders_path, write_to_folders = final_dir(first_level_path+'/'+folder,1)

    write_base_path = export_base_path+'/'+folder

    mri_image_destination_path, mri_image_destination_folders = create_folders((read_base_folders_path, write_to_folders),write_base_path)
    #write_base_path > write_to_folders
    # print(write_base_path,write_to_folders,read_base_folders_path)
    folder_path = read_base_folders_path
    print(folder_path)
    for folder in write_to_folders:
        for image in os.listdir(folder_path+'/'+folder):
            # print(image)
            image_file_with_path = folder_path+'/'+folder+'/'+image
            if(os.path.isfile(image_file_with_path)== False):
                exit()
            try:
                image_data1= load(image_file_with_path)
                img = image_data1[0]
                plt.imshow(img)
                plt.savefig(write_base_path+'/'+folder+'/'+image+'.jpg')
                plt.close()
                print(write_base_path+'/'+folder+'/'+image+'.jpg')
            except ValueError:
                print ('ValueError: could not convert string to float: \'DataCamp\'')

```
