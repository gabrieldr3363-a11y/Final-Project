#  Recycling and Waste Image Classification

## Overview
## Why I Chose This Project


### The Dataset


## How the Model Works

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
