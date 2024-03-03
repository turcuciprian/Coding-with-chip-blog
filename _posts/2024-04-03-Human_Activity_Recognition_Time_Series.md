---
title: "Human Activity Recognition Time Series"
layout: post
date: 2024-03-03
---

This post is based on, complementing and improving on the tutorial here: [https://machinelearningmastery.com/cnn-models-for-human-activity-recognition-time-series-classification/](https://machinelearningmastery.com/cnn-models-for-human-activity-recognition-time-series-classification/)

[https://machinelearningmastery.com/](https://machinelearningmastery.com/) is a great resource for machine leraning tutorials and the must read knowledgebase for any machine learning practitioners.

This blog post has it’s entire basis on the article mentioned at the beginning, the only things it brings extra to the table are:

- gathering a completely new raw dataset
- practice and example for labelling the data for training
- Processing the labelled raw data for the neural network
- Processing raw data for inference
- Running inference on the trained model with the unlabelled raw data

# The problem we are trying to solve

**In short**: Detecting and counting dumbbell reps using the accelerometer of the apple watch (my series 9 apple watch will be used). 

That’s what this blog post is about and I will not repeat the logic of the original blog post, but when modifications are in order, I will mention them and reason the modification.

# The Data

I made an app that starts recording the accelerometer data on the apple watch when I press a button and stops recording when I press it again, the data being outputted in the console in the folowing format for each line: x,y,z

Then I copy and paste the entire console in a csv file and that is my raw data.

## Labelling the data

In order to train the dataset our data must be labelled and (IMPORTANT!) in order to label the data we must take into consideration the categorical natura of the data in the tutorial. in the tutorial 6 different activity types where found in the data and labelled, thus a categorical_crossentropy loss variable was used when fitting the data to the neural network.

In the example I will give, I will only have one type of data I am trying to  identify and that data will be using for this case the binary_crossentropy loss value when fitting. (hotdog or not hotdog problem).

### **To the point**

 To label the data I will use the TRAINSET tool from [https://trainset.geocene.com/](https://trainset.geocene.com/).

This tool has a few requirements for any data you are trying to label:

1. The CSV file where the data is has to be in a format of 4 columns(series, timestamp, value and label):
    - `series` is a unique name of the time series you are labeling. You can include multiple `series` in a CSV.
    - `timestamp` is a timestamp with time zone in [ISO8601 format](https://en.wikipedia.org/wiki/ISO_8601). For example `2019-03-13T21:11:29+00:00` or `2019-03-13T21:11:29Z`.
    - `value` is a numeric scalar. Any real number is a valid for `value`.
    - `label` is an integer representation of a boolean; `0==FALSE` and `1==TRUE`. It is possible to upload data to TRAINSET that is pre-labeled (i.e. the `label` column does not have to start with all zeros). See the [sample CSV](https://trainset.geocene.com/static/files/sample_trainset.csv).

The description for the columns is taken from [https://trainset.geocene.com/help](https://trainset.geocene.com/help) at the state they where at the time when I wrote this post

### The problem

since my raw data is in x,y,z format with values for each row, I need to choose one of the columns for data points to put in the value. create a series name for all points and a timestamp for each point+ a label.

1. the **series**, can be anything. I will just set it to “dumbbells” for all points rows
2. the **timestamp** is something I should of extracted at least in milliseconds when the accelerometer data was extracted, but since I didn’t I will just create a 1 second incremented difference between the points, starting from 00:00:00 of the day the data was read.
3. the value will be one of the x or y or z column from the raw dataset. More on that after this section
4. the label I will set to 0 for all points, as the TRAINSET app will need to have 2 labels:
    1. one for not what I want to highlight as a rep (not hotdog)
    2. one for what I want to highlight as a rep (hotdog!)

### The solution

A script that does the conversion and formatting of the raw data into the TRAINSET required format. Since part of the problem was choosing the right column, x,y,z, I decided to try them all and see what looks more obvious so that I may use the tool to label accurately. 

The column I found more relevant was the y column, where the dumbbells where showing like valleys:

![Untitled](/assets/2024-04-03-Human_Activity_Recognition_Time_Series/Untitled.png)

To this dataset I would just create an extra label ”1” and select all the valleys with tha tlabel active, and export the result at the end.

Problem is, now the value column needs to replace the y column in the original raw data, and the label column must replace all the original 0 values with appropriately labelled 1 and 0 values.

A script that uses the labelled csv file and the original raw data was created and this script would generate a new csv file with x,y,z,label columns that where going to become one training set.

Because for training we need more than just a 10-50 reps dataset I made the second script take all the (labelled) csv files from 1 folder of files with sets of reps, and the result was one **train_all.csv** file with all the data from each files concatenated as 4 large columns of x,y,z,label

**train_all.csv** is the file that is going to be loaded for processing before entering the data into the neural network.

# Understanding Data Preprocessing

The source of our data for training is the **train_all.csv**  that has the columns x,y,z,label with the appropriate labelling. What needs to be done here is to take equal length chunks of points of the data and create objects with those points.

### Why equal length?

The Convolutional neural network expects a fixed number of points in the input and should provide the same length of points in the output. It expects a vector of points of the same length when learning to better and easier provide a better accuracy in the final trained model. In that regard, the input of the data must be a fixed length number of points.

### Choosing input points number length

When I chose the number I took into account the next section “windowing”, as some dumbell reps that are made slower, create a valley that’s larger visually and has many points, and some, that are faster (with less heavy weights or stronger users) will create valleys that are narrower and with less points.

If I choose a maximum size width valley (with possibly many points), when I do inference, if the inference data has let’s say 2 or more narrow valleys that can fit in that large valley chunk, then those 3 might not get labelled by the model correctly as valleys or might not get labelled at all. Thus, taking the smallest most narrow valley makes more sense as a chunk length to be processed.

**Choosing the narrowest valley** from the set is done by **choosing the smallest amount of consecutive points labelled with 1** in the training set.

### Windowing

**The next problem** is choosing the start and the beginning of a length (previously chosen as the narrowest valley from the training set), in a way that relevant data is processed by the training model, so that it can learn and identify what a valley is, regarding of the distance between valleys, or valley length in points. But also correctly knowing and learning valley start and end and possibly contents.

Since we choose the narrowest valley in length of points, a large valley will have that length at the beginning, when the graph drops visually, at the end when it rises back up and a lot of chunks in between.

To appropriately cover most of the data, the dataset should be a length of points (the smallest valley) taken from the training dataset with a start position that increments every 2 points. I set the 2 point value as a very small number that will generate a lot of rows of data, to cover as much as possible in as many rows as possible for the hightest accuracy possible. 

**A few notes:** 

- the original tutorial makes windows every 50% of point chunks from the entire dataset. that is less accurate with the amount of data I gathered and labelled, but is still able to accurately train a model very well given these many windows.
- The purpose of the main (source) tutorial is not to count identified chunks(as it is for this one), but to only accurately identify the points that match the labelled datapoints.
- Having 6 different labelled data types, making 128 point windows and training everything is harder. (to match all the types of activities). And achieving a 98% accuracy, is very impressive.

So each row of training data will be a collection of the minimum amount of points detected with the label 1, in the entire dataset and windows separated by a distance of 2 points will be generated from the entire dataset.

Since the initial tutorial is reading the trained, labelled data from different loaded files (accelerometer, total accelerometer, and gyroscope x,y,z sensors) for 6 different labels and everything is structured in files based on the type of data (train or label) and type of sensor (accelerometer, total accelerometer and gyroscope), the ammount and structure of data is pretty clear.

Here’s a preview of how the train dataset is organised(for train and for test data):

![Untitled](/assets/2024-04-03-Human_Activity_Recognition_Time_Series/Untitled%201.png)

![Untitled](/assets/2024-04-03-Human_Activity_Recognition_Time_Series/Untitled%202.png)

All these files contain the 50% point difference windows data for training are loaded in memory and concatenated and then turned into a 3D array before processing.

y_train.txt and y_test.txt are the labels for all of the body/total/body_gyro x, y, z window points. (Attention! no labels! the labels are in the x_train.txt and y_test.txt).

### How is the data processed?

Let me make this quick and say it again: 

All body_acc x,y,z files, total_acc x,y,z files and body_gyro files are concatenated and turned into a 3D list.

Longer explanation with code:

all the file names are loaded in arrays:

```python
**# load a dataset group, such as train or test
def load_dataset_group(group, prefix=''):
 filepath = prefix + group + '/Inertial Signals/'
 # load all 9 files as a single array
 filenames = list()
 # total acceleration
 filenames += ['total_acc_x_'+group+'.txt', 'total_acc_y_'+group+'.txt', 'total_acc_z_'+group+'.txt']
 # body acceleration
 filenames += ['body_acc_x_'+group+'.txt', 'body_acc_y_'+group+'.txt', 'body_acc_z_'+group+'.txt']
 # body gyroscope
 filenames += ['body_gyro_x_'+group+'.txt', 'body_gyro_y_'+group+'.txt', 'body_gyro_z_'+group+'.txt']
 # load input data
 X = load_group(filenames, filepath)
 # load class output
 y = load_file(prefix + group + '/y_'+group+'.txt')
 return X, y**
```

1. The data from them is appended and turned into a 3d array (using dstack) after all data is appended:

```python
# load a list of files and return as a 3d numpy array
def load_group(filenames, prefix=''):
 loaded = list()
 for name in filenames:
 data = load_file(prefix + name)
 loaded.append(data)
 # stack group so that features are the 3rd dimension
 loaded = dstack(loaded)
 return loaded
```

1. After the data is processed it is returned in trainX, trainX variables:

```python
# load all train
	trainX, trainy = load_dataset_group('train', prefix + 'HARDataset/')
	# load all test
	testX, testy = load_dataset_group('test', prefix + 'HARDataset/')
```

1. The labels are just loaded and returned as y. See the load_dataset_group method. The end of it looks like:

```python
# load class output
	y = load_file(prefix + group + '/y_'+group+'.txt')
	return X, y
```

# Neural Network Change

Since the data we are working with has only 1 marked label (1) and the rest of the data is marked with 0, we are no longer dealing with a categorical crossentropy problem but with a binary_crossentropy problem.

Therefore, 2 changes need to take place:

1. When doing the model.compile, the loss parameter needs to change from categorical_crossentropy to binary_crossentropy:
    
    Ex:
    
    From:
    

```python
model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])
```

To:

```python
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])
```

1. Before processing the labels with to_categorical, since we are dealing with a binary output now, we want the labels to stay the same so we need to remove the first 2 lines from this code:

```python
trainy = trainy - 1
testy = testy - 1
# one hot encode y
trainy = to_categorical(trainy)
testy = to_categorical(testy)
```

Train_y and test_y need to be 1 and 0, and by beeing a list of 1 and 0 and removing 1 from the list would make the 0’s -1 and the 1’s 0 and that will train everything with labels of 0 and in the training process each epoch will give 100% accuracy as everything has the same label value of 0 so everything is in it’s training eyes 0. So by keeping the original 1 and 0 labelling will train the network accurately.

# Data Processing Conclusion and steps forward

The entire process is specific and tedious. But necessary to eventually have that data ready for input in the neural network properly, so that the output 

All of the steps of the data processing can be done entirely in memory. From the windowing part, to the concatenation of windows, splitting of the train and test datasets and having everything in a 3D dstack array ready for network input processing.

I have done the logic and created the code to run from the labelled dataset (train_all.csv file) to input ready for neural network, and here are some conclusions:

- It is doable but not easily done since all the data for training needs to be taken care of properly or it will not work.
- Processing the data (mostly the windows) takes a lot of time. not as much as training but still a lot. it could be done separately but still has to be done and loaded properly for training.
- When eventually doing inference, the same process needs to happen from raw data minus the labelling logic

# Inference

The same processing logic needs to happen for unlabelled x,y,z data in order to feed that data to the network for inferencing.

Basically windows need to be created and fed to the inputs of the model, expecting labelled responses in the output.

The only catch is that the output response is for each row of points, so basically each chunk is ran trough the network and will have a label as a output from the model. 

The distance between the windows can and should be greater than the training 2 point distance but the perfect distance should be tested with resulting outputs on a known number of valleys existent in the raw input data. 

It’s relative to the valley length and I have calculated that **23% of the value length** was an optimal number to start incrementing or decrementing from when trying to find the perfect window point distance for each trained model.

In the end I would group all the consecutive labels greater than 90% accuracy into a group (seen as 1 valley), count the number of resulting groups like that and I would have my number.

By incrementing/decremating the number of distance points and reaching the exact valley number on multiple raw inference datasets, I knew I found the correct value.