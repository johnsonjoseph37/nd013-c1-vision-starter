### Project overview
The objective of the project is to train TensorFlow Object Detection API using data from Waymo Open data set, that has video frames of road trips annotated with vehicles, pedestrains, cyclists, etc. 

### Set up
The code from this repository can be run in a docker container build using build/Dockerfile. I had to downgrade some python libraries to make the code work for COCO evaluation metrics. The commands I used to make the code work in the above docker container are documented in build/run_docker.txt

### Dataset
#### Dataset analysis
By looking at some of the images, I see that there is a variety of scenarios, such as night, unclear and occluded objects, that are well annotated.

However, most of the annotatations are for small objects. I wonder how much information can be picked up from them, and whether this will result in failure to detect close by objects, that are going to be bigger in size.

!(images/Capture1.png)

#### Cross validation
I chose to split the files 80/20 for training & validation. Testing will be done on the 3 files that were not downsampled.

### Training
#### Reference experiment
The reference experiment resulted in a total loss of 0.9842 after 25K steps.

!(images/Capture2.png)

#### Improve on the reference
For the second experiment, I tried to change the evaluation metrics from COCO to Pascal VOC, as well as added a random_adjust_brightness augmentation. This resulted in a total loss of 1.841 after 25K steps.

!(images/Capture3.png)

For the third experiment, I reverted back to COCO and added random_adjust_brightness and random_adjust_contrast augmentations. This resulted in a total loss of 3.65 after 25K steps.

!(images/Capture4.png)

So instead of reducing the loss, the augmentations actually increased it.

I then proceeded to making the animation video from the three experiments, and it confirmed that having a lower loss results in better detection. However, the video of the reference config, still was showing very poor results. 