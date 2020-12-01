# Install NVIDIA Driver, CUDA, cuDNN and OpenCV on Ubuntu 20.04

## Overview

- #### Step 1: Install NVIDIA Driver
- #### Step 2: Install CUDA
- #### Step 3: Install cuDNN
- #### Step 4: Install OpenCV
- #### Step 5: Install Darknet
- #### Step 6: Test the Darknet Training Environment

## Install NVIDIA Driver:

- #### Detect the model of your GPU card and the recommended Driver

    * `ubuntu-drivers devices`
    * `sudo add-apt-repository ppa:graphics-drivers/ppa`
    * `sudo apt-get update`
    * `sudo apt-get install nvidia-driver-455`
    
## Install CUDA:

- #### Go to https://developer.nvidia.com/cuda-downloads and follow the instructions according to your OS

    * **Operating System:** Linux
    * **Architecture:** x86_64
    * **Distribution:** Ubuntu
    * **Version:** 20.04
    * **Type**
    * `wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin`
    * `sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600`
    * `wget https://developer.download.nvidia.com/compute/cuda/11.1.1/local_installers/cuda-repo-ubuntu2004-11-1-local_11.1.1-455.32.00-1_amd64.deb`
    * `sudo dpkg -i cuda-repo-ubuntu2004-11-1-local_11.1.1-455.32.00-1_amd64.deb`
    * `sudo apt-key add /var/cuda-repo-ubuntu2004-11-1-local/7fa2af80.pub`
    * `sudo apt-get update`
    * `sudo apt-get -y install cuda`

- #### Add the following in `~/.bashrc`
 
    ```
    #Darknet
    export PATH=/usr/local/cuda-11.1/bin${PATH:+:$PATH}}
    export LD_LIBRARY_PATH=/usr/local/lib:/usr/local/cuda-11.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    ```
    
- #### Check CUDA version by typing `nvcc -V`

## Install cuDNN:

- #### Go to https://developer.nvidia.com/cuDNN and download `cudnn-11.1-linux-x64-v8.0.5.39.tgz`. Once downloaded, untar the file and copy the contents to their respective locations

    * `tar -xzvf cudnn-11.1-linux-x64-v8.0.5.39.tgz`
    * `sudo cp cuda/include/cudnn*.h /usr/local/cuda/include`
    * `sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64`
    * `sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*`

- #### Check cuDNN version by typing `cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2`

## Install OpenCV:

 #### Go to https://developer.nvidia.com/cuDNN and download `cudnn-11.1-linux-x64-v8.0.5.39.tgz`. Once downloaded, untar the file and copy the contents to their respective locations


We can perform the downloading by running a **Jupyter notebook** from a remote server. Please visit https://ljvmiranda921.github.io/notebook/2018/01/31/running-a-jupyter-notebook for more information

## Install Darknet:

- #### Go to https://github.com/AlexeyAB/darknet and download the `zip` file. Once downloaded, unzip the file. Or alternatively, just type `git clone https://github.com/AlexeyAB/darknet` 

    * `cd darknet`
    * `sed -i 's/GPU=0/GPU=1/' Makefile`
    * `sed -i 's/CUDNN=0/CUDNN=1/' Makefile`
    * `sed -i 's/OPENCV=0/OPENCV=1/' Makefile`
    * `sed -i 's/CUDNN_HALF=0/CUDNN_HALF=1/' Makefile` (optional)
    * `make -j12`




