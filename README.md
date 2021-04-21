# Install TensorRT & tkDNN on Ubuntu 18.04

## Overview

- #### Step 1: Install TensorRT
- #### Step 2: Install tkDNN
- #### Step 3: Export weights and run the demo

## Dependencies

* CUDA 10.0
* CUDNN 7.5.0
* TENSORRT 7.0.0
* OPENCV 3.2

## Install TensorRT:

Download the deb file

  ```
  sudo dpkg -i nv-tensorrt-repo-ubuntu1804-cuda10.0-trt7.0.0.11-ga-20191216_1-1_amd64.deb
  sudo apt-key add /var/nv-tensorrt-repo-cuda10.0-trt7.0.0.11-ga-20191216/7fa2af80.pub
  sudo apt-get update
  sudo apt-get install tensorrt
  sudo apt-get install python3-libnvinfer-dev
  dpkg -l | grep TensorRT (Verify the installation)
  ```

## Install tkDNN:

  ```
  sudo apt install libyaml-cpp-dev
  sudo apt install libeigen3-dev
  ```

* copy some necessary files

  ```
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn.hpp /usr/include/opencv2/
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn/dnn.hpp /usr/include/opencv2/dnn
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn/dict.hpp /usr/include/opencv2/dnn
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn/blob.hpp /usr/include/opencv2/dnn
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn/blob.inl.hpp /usr/include/opencv2/dnn
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn/layer.hpp /usr/include/opencv2/dnn
  sudo cp /home/nechk/OpenCV/opencv_contrib-3.2.0/modules/dnn/include/opencv2/dnn/dnn.inl.hpp /usr/include/opencv2/dnn
  ```

* comment the line here [comment](https://github.com/ceccocats/tkDNN/blob/master/src/Int8BatchStream.cpp#L129)

  ```
  //m_LetterboxImage = cv::dnn::blobFromImage(m_LetterboxImage);
  ```

* compile this [repo](https://github.com/ceccocats/tkDNN)

  ```
  git clone https://github.com/ceccocats/tkDNN
  cd tkDNN
  mkdir build
  cd build
  cmake .. 
  make -j20
  ```

## Export weights and run the demo

```
    test_nn
        |---- layers/ (folder containing a binary file for each layer with the corresponding wieghts and bias)
        |---- debug/  (folder containing a binary file for each layer with the corresponding outputs)
```

* export weights from darknet

  ```
  git clone https://git.hipert.unimore.it/fgatti/darknet.git  
  cd darknet
  make -j20
  mkdir layers debug
  ./darknet export <path-to-cfg-file> <path-to-weights> layers
  ```

  * N.b. Use compilation with CPU (leave GPU=0 in Makefile) if you also want debug

* put `debug` and `layers` in `tkDNN/build/yolo4/`

  * need to modify `tkDNN/tests/darknet/yolo4.cpp` for custom dataset (cfg, names etc)

* check

  ```
  cmake .. -DDEBUG=True
  make -j20
  ```

* create the .rt file by running

  ```
  rm yolo4_fp32.rt        # be sure to delete old tensorRT files
  ./test_yolo4            
  ```

* run the demo

  ```
  ./demo yolo4_fp32.rt ../demo/yolo_test.mp4 y
  ```

  * N.b. By default it is used FP32 inference



-

## References:

- #### [tkDNN](https://github.com/ceccocats/tkDNN)
- #### [tkDNN](https://github.com/ioir123ju/tkDNN)
- #### [csdn](https://blog.csdn.net/gdfsy123/article/details/113823771)
- #### [nvidia](https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html)
- #### [tutorial](https://medium.com/ching-i/tensorrt-%E4%BB%8B%E7%B4%B9%E8%88%87%E5%AE%89%E8%A3%9D%E6%95%99%E5%AD%B8-45e44f73b25e)


