---
title: "Detecting License plates on cars from dashcam videos"
layout: draft
date: 2023-07-04
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

#### Model information:

#### Issues and solutions

1.


#### Problems and solutions

### OCR Model:

Model used:

#### Problems and solutions
