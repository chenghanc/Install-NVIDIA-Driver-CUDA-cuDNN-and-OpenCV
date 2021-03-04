# Install the second CUDA Toolkit and cuDNN on the same machine (Ubuntu 18.04)

## Overview

- #### Step 1: Install the second CUDA Toolkit (10.1)
- #### Step 2: Install the second cuDNN (8.0.5)
- #### Step 3: Configure LD_LIBRARY_PATH

## Install CUDA (10.1):

- #### Go to https://developer.nvidia.com/cuda-downloads and follow the instructions according to your OS

    * **Operating System:** 
      * `Linux`
    * **Architecture:** 
      * `x86_64`
    * **Distribution:** 
      * `Ubuntu`
    * **Version:** 
      * `18.04`
    * **Type**
    * `sudo dpkg -i cuda-repo-ubuntu1804-10-1-local-10.1.105-418.39_1.0-1_amd64.deb`
    * `sudo apt-key add /var/cuda-repo-10-1-local-10.1.105-418.39/7fa2af80.pub`
    * `sudo apt-get update`
    * `sudo apt-get install cuda`

By default, cuda libraries are installed in **/usr/local**. After installing all the CUDA versions you will find a respective folders for each one of the version with the name cuda pointing to the latest installed CUDA toolkit. In my case, I have installed versions 10.0 and 10.1, so my **/usr/local** lists the following:

    ```
    ...
    cuda -> cuda-10.1
    cuda-10.0
    cuda-10.1
    ...
    ```





- #### Add the following in `~/.bashrc`
 
    ```
    #Darknet
    export PATH=/usr/local/cuda-11.1/bin${PATH:+:$PATH}}
    export LD_LIBRARY_PATH=/usr/local/lib:/usr/local/cuda-11.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    ```
    
- #### Check CUDA version by typing 

    * `nvcc -V`

## Install cuDNN:

- #### Go to https://developer.nvidia.com/cuDNN and download `cudnn-11.1-linux-x64-v8.0.5.39.tgz`. Once downloaded, untar the file and copy the contents to their respective locations

    * `tar -xzvf cudnn-11.1-linux-x64-v8.0.5.39.tgz`
    * `sudo cp cuda/include/cudnn*.h /usr/local/cuda/include`
    * `sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64`
    * `sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*`

- #### Check cuDNN version by typing 

    * `cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2`

## Install OpenCV from the source:

- #### Install dependencies

  ```
  sudo apt install build-essential cmake git pkg-config libgtk-3-dev \
    libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
    libxvidcore-dev libx264-dev libjpeg-dev libpng-dev libtiff-dev \
    gfortran openexr libatlas-base-dev python3-dev python3-numpy \
    libtbb2 libtbb-dev libdc1394-22-dev libopenexr-dev \
    libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev
  ```
    
- #### Clone the OpenCV and OpenCV contrib repositories

    * `mkdir ~/opencv_build && cd ~/opencv_build`
    * `git clone https://github.com/opencv/opencv.git`
    * `git clone https://github.com/opencv/opencv_contrib.git`
     
- #### Create a temporary build directory

    * `cd ~/opencv_build/opencv`
    * `mkdir -p build && cd build`
     
- #### Set up the OpenCV build with CMake
 
  ```
  cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_GENERATE_PKGCONFIG=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_build/opencv_contrib/modules \
    -D BUILD_EXAMPLES=ON ..
  ```

    
- #### Compilation

    * Start the compilation process by typing 
      * `make -j8`
    * Install OpenCV with 
      * `sudo make install`  
    * To verify the installation by typing 
      * `pkg-config --modversion opencv4`
      * `4.5.1`
    * Please visit https://linuxize.com/post/how-to-install-opencv-on-ubuntu-20-04/ for more information

## Install Darknet:

- #### Go to https://github.com/AlexeyAB/darknet and download the `zip` file. Once downloaded, unzip the file. Or alternatively, just type `git clone https://github.com/AlexeyAB/darknet` 

    * `cd darknet`
    * `sed -i 's/GPU=0/GPU=1/' Makefile`
    * `sed -i 's/CUDNN=0/CUDNN=1/' Makefile`
    * `sed -i 's/OPENCV=0/OPENCV=1/' Makefile`
    * `sed -i 's/CUDNN_HALF=0/CUDNN_HALF=1/' Makefile` (optional)
    * `make -j12`

- #### Remarks

    * The installation is tested on GeForce RTX 2080 Super
    * Need to remove older compute capability: **`-gencode arch=compute_30,code=sm_30`**
    * Add new compute capability: **`ARCH= -gencode arch=compute_75,code=[sm_75,compute_75]`**

## Test the Darknet Environment

- #### We can test a single image by running the command

    * `./darknet detector test cfg/coco.data cfg/yolov4.cfg ../weights/yolov4.weights -thresh 0.25 -dont_show data/dog.jpg`
    
- #### If everything works fine, it will show something like this at beginning and end

  ```
  CUDA-version: 11010 (11010), cuDNN: 8.0.5, GPU count: 1  
  OpenCV version: 4.5.1
  0 : compute_capability = 750, cudnn_half = 0, GPU: GeForce RTX 2080 Super with Max-Q Design 
  ```
  
  ```
  Done! Loaded 162 layers from weights-file 
  Detection layer: 139 - type = 28 
  Detection layer: 150 - type = 28 
  Detection layer: 161 - type = 28 
  data/dog.jpg: Predicted in 361.098000 milli-seconds.
  bicycle: 92%
  dog: 98%
  truck: 92%
  pottedplant: 33%
  ```

- #### We can start Training on custom dataset by running the command

    * `./darknet detector train sun.data sun.cfg yolov4-tiny.conv.29 -map -dont_show`

- #### Train on a remote server and watch mAP & Loss chart using local machine browser

    * Log-in to remote machine via an ssh command `ssh nechk@192.168.1.117`
    * Train on custom dataset by running 
      * `./darknet detector train sun.data sun.cfg yolov4-tiny.conv.29 -map -dont_show -mjpeg_port 8090 > log.txt`
    * In your remote machine, the training uses the port XXXX=8090 which you specified
    * To watch mAP & Loss chart from your remote machine, type `http://192.168.1.117:8090/`
    
    
