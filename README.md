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

## Set PATH

```shell
echo "PATH=/usr/share/tesseract-ocr/bin:\$PATH" >> ~/.profile

source ~/.profile
```

## Check Version

```shell
tesseract --version

or

/usr/share/tesseract-ocr/bin/tesseract --version
```

Screen shot

```
tesseract 3.05.02
 leptonica-1.75.3
  libgif 5.1.4 : libjpeg 8d (libjpeg-turbo 1.5.2) : libpng 1.6.34 : libtiff 4.0.9 : zlib 1.2.11 : libwebp 0.6.1 : libopenjp2 2.3.0
```

