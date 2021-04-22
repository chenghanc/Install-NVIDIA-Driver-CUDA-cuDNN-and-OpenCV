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

  * need to modify `tkDNN/tests/darknet/yolo4.cpp` for custom dataset (cfg, names)

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

* **FP16 inference**

  run an object detection demo with FP16 inference

  ```
  export TKDNN_MODE=FP16  # set the half floating point optimization
  rm yolo4_fp16.rt        # be sure to delete old tensorRT files
  ./test_yolo4
  ./demo yolo4_fp16.rt ../demo/yolo_test.mp4 y
  ```

  * N.b. Using FP16 inference will lead to some errors in the results (first or second decimal)

* **Inference tkDNN + Tensorrt with Python**

  * compile this [repo](https://github.com/ioir123ju/tkDNN) (same as above)

  ```
  git clone https://github.com/ioir123ju/tkDNN tkDNN2
  cd tkDNN2
  mkdir build
  cd build
  cmake ..
  make -j20

  //m_LetterboxImage = cv::dnn::blobFromImage(m_LetterboxImage);
  ```

  ```
  cmake .. -DDEBUG=True
  make -j20

  ./test_yolo4
  ./demo yolo4_fp32.rt ../demo/yolo_test.mp4 y
  ```

  * run an object detection demo with python

  ```
  python darknetTR.py build/yolo4_fp32.rt --video=demo/yolo_test.mp4
  ```

## Export weights and run the demo (Cart)

* export weights from darknet

  ```
  ./darknet export ../YoloFastest/cart/cart-tinyrcorr.cfg ../YoloFastest/cart/cart-tinyrcorr_best.weights layers
  ```

* put `debug` and `layers` in `tkDNN/build/yolo4tiny`

* need to modify `tkDNN/tests/darknet/yolo4tiny.cpp` for custom dataset (cfg, names)

  ```
  std::string cfg_path  = std::string(TKDNN_PATH) + "/../YoloFastest/cart/cart-tinyrcorr.cfg";
  std::string name_path = std::string(TKDNN_PATH) + "/../sddownload/cart.names";
  ```

* check

  ```
  cmake .. -DDEBUG=True
  make -j20
  ```

* create the .rt file by running

  ```
  ./test_yolo4tiny
  ```

* run the demo

  ```
  ./demo yolo4tiny_fp32.rt test.mp4 y
  ```

## Export weights and run the demo (Head)

* export weights from darknet

  ```
  ./darknet export ../headv1corr.cfg ../headv1corr_9000.weights layers/
  ```

* put `debug` and `layers` in `tkDNN/build/yolo4`

* need to modify `tkDNN/tests/darknet/yolo4.cpp` for custom dataset (cfg, names)

  ```
  std::string cfg_path  = std::string(TKDNN_PATH) + "/../headv1corr.cfg";
  std::string name_path = std::string(TKDNN_PATH) + "/../../../baby.names";
  ```

* check

  ```
  cmake .. -DDEBUG=True
  make -j20
  ```

* create the .rt file by running

  ```
  ./test_yolo4
  ```

* run the demo

  ```
  ./demo yolo4_fp32.rt test.mp4 y
  ```

<details>
  <summary>Output (FPS)</summary>

```
  detection
  yolo4_fp32-head.rt
  New NetworkRT (TensorRT v7)
  Float16 support: 1
  Int8 support: 1
  DLAs: 0
  TENSORRT LOG: Deserialize required 3193145 microseconds.
  create execution context
  TENSORRT LOG: Current optimization profile is: 0. Please ensure there are no enqueued operations pending in this context prior to switching profiles
  Input/outputs numbers: 4
  input index = 0 -> output index = 3
  Data dim: 1 3 512 512 1
  Data dim: 1 33 16 16 1
  RtBuffer 0   dim: Data dim: 1 3 512 512 1
  RtBuffer 1   dim: Data dim: 1 33 64 64 1
  RtBuffer 2   dim: Data dim: 1 33 32 32 1
  RtBuffer 3   dim: Data dim: 1 33 16 16 1
  camera started
  detection end

  Time stats:
  Min: 9.41671 ms
  Max: 17.5369 ms
  Avg: 10.438 ms  95.8034 FPS
```

</details>

<details>
  <summary>Output Tiny (FPS)</summary>

```
  detection
  yolo4tiny_fp32-head.rt
  New NetworkRT (TensorRT v7)
  Float16 support: 1
  Int8 support: 1
  DLAs: 0
  TENSORRT LOG: Deserialize required 970329 microseconds.
  create execution context
  TENSORRT LOG: Current optimization profile is: 0. Please ensure there are no enqueued operations pending in this context prior to switching profiles
  Input/outputs numbers: 4
  input index = 0 -> output index = 3
  Data dim: 1 3 640 640 1
  Data dim: 1 55 80 80 1
  RtBuffer 0   dim: Data dim: 1 3 640 640 1
  RtBuffer 1   dim: Data dim: 1 44 20 20 1
  RtBuffer 2   dim: Data dim: 1 55 40 40 1
  RtBuffer 3   dim: Data dim: 1 55 80 80 1
  camera started
  detection end

  Time stats:
  Min: 2.94812 ms
  Max: 5.8546 ms
  Avg: 3.28019 ms 304.861 FPS
```

</details>

## References:

- #### [tkDNN](https://github.com/ceccocats/tkDNN)
- #### [tkDNN](https://github.com/ioir123ju/tkDNN)
- #### [Inference tkDNN + Tensorrt with Python](https://github.com/ceccocats/tkDNN/issues/30)
- #### [C++](https://github.com/ceccocats/tkDNN/issues/87)
- #### [csdn](https://blog.csdn.net/gdfsy123/article/details/113823771)
- #### [nvidia](https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html)
- #### [tutorial](https://medium.com/ching-i/tensorrt-%E4%BB%8B%E7%B4%B9%E8%88%87%E5%AE%89%E8%A3%9D%E6%95%99%E5%AD%B8-45e44f73b25e)


