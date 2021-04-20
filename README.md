# Install TensorRT & tkDNN on Ubuntu 18.04

## Overview

- #### Step 1: Install TensorRT
- #### Step 2: Install tkDNN

## Dependencies

- #### Ubuntu 18.04
- #### CUDA 10.0
- #### CUDNN 7.5.0
- #### TENSORRT 7.0.0
- #### OPENCV 3.2

## Install TensorRT:

- #### `sudo dpkg -i nv-tensorrt-repo-ubuntu1804-cuda10.0-trt7.0.0.11-ga-20191216_1-1_amd64.deb`
- #### `sudo apt-key add /var/nv-tensorrt-repo-cuda10.0-trt7.0.0.11-ga-20191216/7fa2af80.pub`
- #### `sudo apt-get update`
- #### `sudo apt-get install tensorrt`
- #### `sudo apt-get install python3-libnvinfer-dev`
- #### `dpkg -l | grep TensorRT` (Verify the installation)

## Install tkDNN:

- #### `sudo apt install libyaml-cpp-dev`
- #### `sudo apt install libeigen3-dev`
- #### copy some necessary files

  ```
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn.hpp /usr/include/opencv2/
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn/dnn.hpp /usr/include/opencv2/dnn
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn/dict.hpp /usr/include/opencv2/dnn
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn/blob.hpp /usr/include/opencv2/dnn
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn/blob.inl.hpp /usr/include/opencv2/dnn
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn/layer.hpp /usr/include/opencv2/dnn
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn/dnn.inl.hpp /usr/include/opencv2/dnn
  ```

- #### comment the line here [comment](https://github.com/ceccocats/tkDNN/blob/master/src/Int8BatchStream.cpp#L129)

  ```
  //m_LetterboxImage = cv::dnn::blobFromImage(m_LetterboxImage);
  ```
- #### compile this [repo](https://github.com/ceccocats/tkDNN)

  ```
  git clone https://github.com/ceccocats/tkDNN
  cd tkDNN
  mkdir build
  cd build
  cmake .. 
  make -j20
  ```

-

## References:

- #### [tkDNN](https://github.com/ceccocats/tkDNN)
- #### [nvidia](https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html)
- #### [tutorial](https://medium.com/ching-i/tensorrt-%E4%BB%8B%E7%B4%B9%E8%88%87%E5%AE%89%E8%A3%9D%E6%95%99%E5%AD%B8-45e44f73b25e)


