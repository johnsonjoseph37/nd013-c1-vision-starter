### Project overview
The objective of the project is to train TensorFlow Object Detection API using data from Waymo Open data set, that has video frames of road trips annotated with vehicles, pedestrains, cyclists, etc. 

### Set up
The code from this repository can be run in a docker container build using build/Dockerfile. I had to downgrade some python libraries to make the code work for COCO evaluation metrics. The commands I used to make the code work in the above docker container are documented in build/run_docker.txt

### Dataset
#### Dataset analysis
By looking at some of the images, I see that there is a variety of scenarios, such as night, unclear and occluded objects, that are well annotated.

However, most of the annotatations are for small objects. I wonder how much information can be picked up from them, and whether this will result in failure to detect close by objects, that are going to be bigger in size.

![Distribution Analysis](https://user-images.githubusercontent.com/84423466/143522844-14001892-8b55-4094-9130-5c0e4fd2b1c7.png)

#### Cross validation
I chose to split the files 80/20 for training & validation. Testing will be done on the 3 files that were not downsampled.

### Training
#### Reference experiment
The reference experiment resulted in a total loss of 0.9842 after 25K steps.

![Reference Experiment](https://user-images.githubusercontent.com/84423466/143522939-1ef3bad8-53fa-4145-8f2e-cdc3566e5d8c.png)

#### Improve on the reference
For the second experiment, I tried to change the evaluation metrics from COCO to Pascal VOC, as well as added a random_adjust_brightness augmentation. This resulted in a total loss of 1.841 after 25K steps.

![Experiment #1](https://user-images.githubusercontent.com/84423466/143523005-07f03c55-32a0-4d15-8c34-3e51107c2639.png)

For the third experiment, I reverted back to COCO and added random_adjust_brightness and random_adjust_contrast augmentations. This resulted in a total loss of 3.65 after 25K steps.

![Experiment #2](https://user-images.githubusercontent.com/84423466/143523027-60a357e5-be7d-42f3-97c0-a228d2895c7c.png)

So instead of reducing the loss, the augmentations actually increased it.

I then proceeded to making the animation video from the three experiments, and it confirmed that having a lower loss results in better detection. However, the video of the reference config, still was showing very poor results. 
