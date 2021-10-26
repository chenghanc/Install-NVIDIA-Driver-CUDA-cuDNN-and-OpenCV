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

## Build training tools

```shell
make training
sudo make training-install
```

## Language Data

- Download the data files `https://github.com/tesseract-ocr/tessdata/tree/3.04.00`

```shell
unzip tessdata-3.04.00.zip
mv tessdata-3.04.00 tessdata
sudo mv tessdata/* /usr/share/tesseract-ocr/share/tessdata/
export TESSDATA_PREFIX=/usr/share/tesseract-ocr/share/tessdata
```

## Install Qt-box-editor

- Go to the github [zdenop/qt-box-editor](https://github.com/zdenop/qt-box-editor) and download the latest released source code
- Compile the source code

```shell
BUILDING ON UBUNTU
==================

sudo apt-get install qt5-qmake
sudo apt-get install qt5-default
sudo apt-get install libqt5gui5
sudo apt-get install qt5-doc
sudo apt-get install libqt5svg5-dev

qmake
make -j20
```

- If compilation fails, you can try to uninstall Leptonica `sudo apt-get remove libleptonica-dev` and install [leptonica version 1.78.0](https://distfiles.macports.org/leptonica/) from source. Open a shell and execute (Optional)

```shell
./configure
sudo make -j20
sudo make install
```

- Also, the system might somehow mixup `conda qt` and `apt-get qt`. One solution is to exchange to another conda environment, which has no qt installation (say, qt 5.9.7). See [link](https://github.com/zdenop/qt-box-editor/issues/77) for more informations

- Open Qt-box-editor by executing `./release/qt-box-editor-1.12rc1`


## Install jTessBoxEditor

- Go to the download page and download [jTessBoxEditor-2.3.1.zip](https://sourceforge.net/projects/vietocr/files/jTessBoxEditor/)

- unzip the file and activate jTessBoxEditor by executing `java -Xms128m -Xmx1024m -jar jTessBoxEditor.jar`

## Training Tesseract

Please see [Training-Tesseract-3.03–3.05](https://github.com/tesseract-ocr/tessdoc/blob/main/tess3/Training-Tesseract-3.03%E2%80%933.05.md) and [Tutorial](https://www.jianshu.com/p/5f847d8089ce)



## Uninstall Tesseract

- Run `sudo make uninstall` in the `Tesseract` folder
- Remove the `Tesseract` folder

---

## References

1. [Ubuntu 18.04 krissdap/Code](https://gist.github.com/krissdap/d888a50a9212f5da3ce5e5c80c553831)
2. [Ubuntu 16.04](https://blog.csdn.net/u011807371/article/details/76178480)
3. [Tesseract documentation](https://tesseract-ocr.github.io/tessdoc/Compiling.html#linux)
4. [Leptonica](http://www.leptonica.org/)
5. [tessdata](https://github.com/tesseract-ocr/tessdata/tree/3.04.00)
6. [tessdoc](https://github.com/tesseract-ocr/tessdoc)
7. [astutejoe/tesseract-tutorial](https://github.com/astutejoe/tesseract-tutorial)
8. [Shreeshrii/tessdata_ocrb](https://github.com/Shreeshrii/tessdata_ocrb)
9. [jTessBoxEditor](http://vietocr.sourceforge.net/training.html)
10. [OCRB](https://github.com/brendanjerwin/cold_steel_storage/blob/master/OCRB.ttf)
11. [Training-data](https://pretius.com/blog/ocr-tesseract-training-data/)
12. [Tutorial](https://b98606021.medium.com/%E5%AF%A6%E7%94%A8%E5%BF%83%E5%BE%97-tesseract-ocr-eef4fcd425f0)
13. [Tutorial](https://blog.csdn.net/u011807371/article/details/77164181)
14. [Tutorial](http://gwang-cv.github.io/2017/08/25/Tesseract4.0+jTessBoxEditor%E8%AE%AD%E7%BB%83(ubuntu%E4%B8%8B)/)
15. [Tutorial](https://www.jianshu.com/p/31afd7fc5813)
