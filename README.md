# yolov11-trt

[![cuda](https://img.shields.io/badge/cuda-11.6-green)](https://developer.nvidia.com/cuda-downloads)
[![trt](https://img.shields.io/badge/TRT-8.6-green)](https://developer.nvidia.com/tensorrt)

This repository contains a C++ implementation of YOLOv11, optimized with TensorRT for fast and efficient real-time inference.

### Dependencies
- **C++**:
    - OpenCV
    - TensorRT

- **Python**:
  
  ```bash
  pip install --upgrade ultralytics
  ```

### Installation

#### Repository clone & build

```bash
git clone https://github.com/fbasatemur/yolov11-trt.git
cd yolov11-trt*
```
```bash
mkdir build && cd build
cmake ..
cmake --build . --config Release
```

## Usage

### Exporting the Model


Modify the export.py script for the desired model name, then run it to export the YOLOv11 model to ONNX.

```bash
python export.py
```


### Create a TRT Engine

Convert the ONNX model to a TRT engine:

```bash
./yolov11-trt yolo11.onnx "yolov11.engine"
```


### Running Inference

Perform object detection on an image:

```bash
./yolov11-trt yolo11.engine "img1.jpg"
```

Perform object detection on a video:

```bash
./yolov11-trt yolo11.engine "vid1.mp4"
```

#### References

- https://github.com/spacewalk01/yolov11-tensorrt