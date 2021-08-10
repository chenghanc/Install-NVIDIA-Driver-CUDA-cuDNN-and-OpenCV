# Install the dependencies of tensorrtx on Ubuntu 18.04

## 1. Install CUDA
## 2. Install TensorRT
## 3. Install OpenCV
## 4. Check your installation

```
dpkg -l | grep cuda
dpkg -l | grep nvinfer
dpkg -l | grep opencv
```

## Dependencies

* CUDA 10.0
* CUDNN 7.5.0
* TENSORRT 7.0.0
* OPENCV 3.2

## yolov5

* Number of classes defined in yololayer.h
* Input width and height defined in yololayer.h
* Comment the lines here [comment](https://github.com/wang-xinyu/tensorrtx/blob/master/yolov5/calibrator.cpp#L52) and [comment](https://github.com/wang-xinyu/tensorrtx/blob/master/yolov5/calibrator.cpp#L54)

  ```
  //cv::Mat blob = cv::dnn::blobFromImages(input_imgs_, 1.0 / 255.0, cv::Size(input_w_, input_h_), cv::Scalar(0, 0, 0), true, false);

  //CUDA_CHECK(cudaMemcpy(device_input_, blob.ptr<float>(0), input_count_ * sizeof(float), cudaMemcpyHostToDevice));
  ```

* Generate .wts from pytorch with .pt

  ```
  git clone -b v5.0 https://github.com/ultralytics/yolov5.git
  git clone https://github.com/wang-xinyu/tensorrtx.git
  // download https://github.com/ultralytics/yolov5/releases/download/v5.0/yolov5s.pt
  cp {tensorrtx}/yolov5/gen_wts.py {ultralytics}/yolov5
  cd {ultralytics}/yolov5
  python gen_wts.py yolov5s.pt
  // a file 'yolov5s.wts' will be generated.
  ```

* Build tensorrtx/yolov5 and run

  ```
  cd {tensorrtx}/yolov5/
  // update CLASS_NUM in yololayer.h if your model is trained on custom dataset
  mkdir build
  cd build
  cp {ultralytics}/yolov5/yolov5s.wts {tensorrtx}/yolov5/build
  cmake ..
  make
  sudo ./yolov5 -s [.wts] [.engine] [s/m/l/x/s6/m6/l6/x6 or c/c6 gd gw]  // serialize model to plan file
  sudo ./yolov5 -d [.engine] [image folder]  // deserialize and run inference, the images in [image folder] will be processed.
  // For example yolov5s
  sudo ./yolov5 -s yolov5s.wts yolov5s.engine s
  sudo ./yolov5 -d yolov5s.engine ../samples
  // For example Custom model with depth_multiple=0.17, width_multiple=0.25 in yolov5.yaml
  sudo ./yolov5 -s yolov5_custom.wts yolov5.engine c 0.17 0.25
  sudo ./yolov5 -d yolov5.engine ../samples
  ```

* Optional, load and run the tensorrt model in python

  ```
  // install python-tensorrt, pycuda, etc.
  // conda create -n tensorrt python=3.7
  // download TensorRT-7.0.0.11.Ubuntu-18.04.x86_64-gnu.cuda-10.0.cudnn7.6.tar
  // ...
  // pip install tensorrt-7.0.0.11-cp37-none-linux_x86_64.whl
  // pip install pycuda
  // ...
  // ensure the yolov5s.engine and libmyplugins.so have been built
  python yolov5_trt.py
  ```
