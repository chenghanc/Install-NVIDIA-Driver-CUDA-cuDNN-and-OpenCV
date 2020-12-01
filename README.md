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
- #### Operating System: Linux 
- #### Architecture: x86_64 
- #### Distribution: Ubuntu 
- #### Version: 20.04
    
    * `wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin`
    * `sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600`
    * `wget https://developer.download.nvidia.com/compute/cuda/11.1.1/local_installers/cuda-repo-ubuntu2004-11-1-local_11.1.1-455.32.00-1_amd64.deb`
    * `sudo dpkg -i cuda-repo-ubuntu2004-11-1-local_11.1.1-455.32.00-1_amd64.deb`
    * `sudo apt-key add /var/cuda-repo-ubuntu2004-11-1-local/7fa2af80.pub`
    * `sudo apt-get update`
    * `sudo apt-get -y install cuda`

- #### Add following in `~/.bashrc`
 
    ```
    #Darknet
    export PATH=/usr/local/cuda-11.1/bin${PATH:+:$PATH}}
    export LD_LIBRARY_PATH=/usr/local/lib:/usr/local/cuda-11.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    ```
    
    

## Install cuDNN:

## Install OpenCV:

We can perform the downloading by running a **Jupyter notebook** from a remote server. Please visit https://ljvmiranda921.github.io/notebook/2018/01/31/running-a-jupyter-notebook for more information

## Usage:

- #### Step 1: Run Jupyter Notebook from remote machine

    * Log-in to your remote machine via `ssh` command. Type the following
    *

  ```ini
  $ jupyter notebook --no-browser --port=8888
  ```

- #### Step 2: Forward port XXXX to YYYY and listen to it

    * In your remote machine, the notebook is running at the port XXXX=8888 which you specified
    * Forward port XXXX=8888 to port YYYY=8889 of your local machine so that you can listen and run it from your browser
    *
   
  ```
  $ ssh -N -f -L localhost:8889:localhost:8888 nechk@192.168.1.117
  ```

- #### Step 3: Open Jupyter Notebook

    * To open the Jupyter notebook from your remote machine
    * Type http://localhost:8889/

