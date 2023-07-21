---
title: "Detecting License plates on cars from dashcam videos"
date: 2023-07-15
---

## The idea:

The final "product" needs to be a way to extract all the unique license plates from cars in a dash cam video and placed into a CSV file, stacked on top of eachother.

It's a Simple idea. But is it a useful one?

Well, if you have all the license platest you passed on the street in a CSV file you get to do what ever you want with them:

- Search for a specific one
- Match them with license plates you know and figure out who passed by you on the street.
- If you're law inforcement and want to identify certain license plates from your dash cam video.

... and [ insert your use case here ]

## The implementation:

I approached this as a machine learning problem that can be solved with 2 models:

- a license plate recognition model
- an OCR model

Both models must work well individually and must be made to work together by feeding the results from the license plate recognition to the OCR model.

### License plate recognition model:

Model used:

#### Model information:

### OCR Model:

Model used:

#### Model information:

#### Issues and solutions

1. Identifying the license plates better
    **Problem**:
        The bounding boxes around the license plates are not perfectly aligned around the license plates, so I cannot properly identify the region of the license plate for later cropping and processing
    **Solution**:
        The first time a license plate region is predicted, I add 20 px to the bounding box, (top, bottom, left and right), crop the new region and re-run the license plate inference on the cropped image. The new detection of the edges of the license plate will be more accurate on the second inference.
2. to save or not to save the result?
    **Problem**:
        The best identified bounding box from Solution #1 is a new cropped image. I will need to run an OCR model for text detection on this. Should I save it as a file or should I process it from memory?
    **Solution**:
        Processing it from memory is the way to go, but also saving it alongside the original image inside a new folder would also be a good idea for later visualisation (original file base name)
3. What do I run the OCR on? Image file or image memory?
    **Problem**: 
        Running the OCR on the image file requires extra steps and more memory and processing power. Running it in memory is faster and more efficient for processing many images.
    **Solution**: 
        Since it's faster and more efficient because the script does not have to save the file, open it to process it, just process it it's a good idea just to process it. But we are also Saving the file. The only gain in efficiency would be that we don't have to open the file to process it with OCR.
4. Why do I run OCR on it and what do I do after?
    **Problem**:
        Ok, OCR returns some text, what do I do with it?
    **Solution**:
        Each text is: 
            1. Checked for length if it qualifies as a license plate 
            2. I applied a regex to match the license plates for my country
            2. If it qualifies, it should be appended to a csv file if it does not exist, alongside the original name for the file it was extracted from (the full image not the cropped version)




