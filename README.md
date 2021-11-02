# Install Tesseract 5 on Ubuntu 18.04


## Uninstall Tesseract 3

- Run `sudo make uninstall` in the `Tesseract` folder
- Remove the `Tesseract` folder
- Run `sudo rm -rf /usr/share/tesseract-ocr/`

## Download Tesseract OCR 5 source file

[tesseract-5.0.0-rc1.tar](https://github.com/tesseract-ocr/tesseract/releases)

## Pre-requisites

```shell
sudo apt-get install g++
sudo apt-get install autoconf automake libtool
sudo apt-get install pkg-config
sudo apt-get install libpng-dev
sudo apt-get install libjpeg8-dev
sudo apt-get install libtiff5-dev
sudo apt-get install zlib1g-dev
```

## Pre-requisites for training tools (Optional)

```shell
sudo apt-get install libicu-dev
sudo apt-get install libpango1.0-dev
sudo apt-get install libcairo2-dev
```

## Install Developer Tools used for training

```shell
sudo apt install libtesseract-dev
```

## Install Leptonica [link](https://github.com/chenghanc/Install-NVIDIA-Driver-CUDA-cuDNN-and-OpenCV/tree/tesseract3)

## Compile and run

```shell
./autogen.sh
./configure --prefix=/usr/share/tesseract-ocr/
make -j20
sudo make install
sudo ldconfig
```

## Set PATH (Optional)

```shell
echo "PATH=/usr/share/tesseract-ocr/bin:\$PATH" >> ~/.profile

source ~/.profile
```

## Check Version

```shell
tesseract --version
```

## Build training tools

```shell
make training
sudo make training-install
```

## Language Data

- Download the data files `https://github.com/tesseract-ocr/tessdata`

```shell
unzip tessdata.zip
sudo mv tessdata/* /usr/share/tesseract-ocr/share/tessdata/
export TESSDATA_PREFIX=/usr/share/tesseract-ocr/share/tessdata
```

## Training Tesseract (Generate Training Images and Box Files)

- **Step 0:** Activate `jTessBoxEditor`

- **Step 1:** Generate




---

## References

