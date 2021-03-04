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
  cuda -> cuda-10.1
  cuda-10.0
  cuda-10.1
  ```

## Install cuDNN (8.0.5):

- #### Go to https://developer.nvidia.com/cuDNN and download `cudnn-10.1-linux-x64-v8.0.5.39.tgz`. Once downloaded, untar the file and copy the contents to their respective locations

    * `tar xvf cudnn-10.1-linux-x64-v8.0.5.39.tgz`
    * `sudo cp cuda/include/cudnn* /usr/local/cuda/include/`
    * `sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/`
    * `sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*`

- #### Check cuDNN version by typing 

    * `cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2`

## Configure LD_LIBRARY_PATH by adding the following in `~/.bashrc`
 
    ```
    # DARKNET
    export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}
    #export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:/usr/local/cuda-10.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    ```

- #### Please visit https://gist.github.com/raulqf/1733e64abf5c850805f32dd3d0f13c32 for more information
