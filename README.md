#  Vehicle Classification

## Overview
This model classifies 7 types of vehicles from cars to planes. This can be used in the cameras of AI vehicles to identify if the vehicle it sees is in a safe lane or not. Like if an AI vehicle sees a plane flying a few miles infront of them, they should know that theres probably an airport over there. This is extremely important for future AI vehicles.
## Why I Chose This Project
I chose this project because as AI cars become a reality, they should know basic common sense of humans. While they might have better navigation and accurate GPS, without common sense AI cars will fail horribly and it would be better if humans drove the cars. Ai cars should be able to identify the main vehicles and know how to travel so that the car doesn't crash into a bike, or run straight into a train. These are extreme examples but show a real danger of AI cars. 

### The Dataset
My dataset included 7 categories of bikes, cars, motorcycles, rickshaws, ships, trains, and planes. My training had around 700 images for each category, my val had around 50 images, and my test had around 50 images. In total the dataset was made of around 5600 images. 
bikes: normal bicycles, electric bikes, little kid bikes
cars: old cars, new cars, luxury cars, pick up trucks
motorcycles: chopper motorcycles, sport motorcycles
rickshaws: taxis, tourist attraction
ships: cruises, cargo ships, sailboat
planes: jets, commercial airliners, cargo planes

## How the Model Works
I built the model using a pre-trained neural network and used a dataset to train it to classify vehicles based on the images it was given. The model was given this data set because it included many pictures of vehicles in different lighting, position, and surroundings, which made the real model better at classifying real world situations. The model will output a simple classification: Car, Bike, Motorcycle, Rickshaw, Ship, or Plane, along with a confidence rating depending on how close the image is to its training data. 


## Setup
### 1. Install Jetson Inference

```
git clone --recursive https://github.com/dusty-nv/jetson-inference
cd jetson-inference
mkdir build
cd build
cmake ../
make
sudo make install
```

### 2. Prepare Dataset

Organize images like this:
```
jetson-inference/python/training/classification/data/vehicle_classification/
├── train/
│   ├── car/
│   ├── train/
│   ├── plane/
│   ├── rickshaw/
│   ├── motorcycle/
│   ├── ship/
│   ├── bike/
├── val/
└── test/

```

### 3. Training

1. Enable more memory: `echo 1 | sudo tee /proc/sys/vm/overcommit_memory`
2. Train the model (I used batch size of 4)
  ```
  cd jetson-inference
  ./docker/run.sh
  cd python/training/classification
  python3 train.py --model-dir=models/vehicle_classification data/Data
  ```
3. Export Model
  ```
  # Still in docker container:
  python3 onnx_export.py --model-dir=models/vehicle_classification
  ```

## Using the Model

### Set Variables
```
cd jetson-inference/python/training/classification
NET=models/vehicle_classification
DATASET=data/Data
```

### Test on Image
```
imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/car/<image.jpg> result.jpg
```
Replace <image.jpg> with your actual image.

![output]()

### Live Camera
```
imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt /dev/video0 output.mp4
```

## Resources
* [The Finished Model]()
* [Dataset]()
* [ImageNet Documentation](https://github.com/dusty-nv/jetson-inference/blob/master/docs/imagenet-console-2.md)
* [Jetson Inference GitHub](https://github.com/dusty-nv/jetson-inference)
* [Video Demonstration]()
