# Install Tesseract 3.05.02 on Ubuntu 18.04


## Download Tesseract OCR 3.05.02 source file

```shell
wget https://github.com/tesseract-ocr/tesseract/archive/3.05.02.tar.gz
```

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

## Install Leptonica

```shell
sudo apt-get install libleptonica-dev
```
## Compile and run

```shell
./autogen.sh
./configure --prefix=/usr/share/tesseract-ocr/
make
sudo make install
```

