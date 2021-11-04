# Install Tesseract 5 on Ubuntu 18.04


## Uninstall Tesseract 3

If you have installed Tesseract 3, uninstall it by running the following commands

- Run `sudo make uninstall` in the `Tesseract` folder
- Remove the `Tesseract` folder
- Run `sudo rm -rf /usr/share/tesseract-ocr/`

## Download Tesseract OCR 5 source file

Go to the download page and download the lastest released file

[tesseract-5.0.0-rc1.tar](https://github.com/tesseract-ocr/tesseract/releases)

## Pre-requisites

The following libraries need to be installed

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

The following libraries need to be installed if you plan to install the training tools

```shell
sudo apt-get install libicu-dev
sudo apt-get install libpango1.0-dev
sudo apt-get install libcairo2-dev
```

## Install Developer Tools used for training

Run the following command if you wish to train custom dataset

```shell
sudo apt install libtesseract-dev
```

## Install Leptonica [link](https://github.com/chenghanc/Install-NVIDIA-Driver-CUDA-cuDNN-and-OpenCV/tree/tesseract3)

## Compile and run

Install Tesseract by running the following commands

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

Check version by running the following command

```shell
tesseract --version
```

## Build training tools

Build training tools by running the following commands

```shell
make training
sudo make training-install
```

## Language Data

- Download the data files `https://github.com/tesseract-ocr/tessdata`
- unzip the file and move to relevant folder

```shell
unzip tessdata.zip
sudo mv tessdata/* /usr/share/tesseract-ocr/share/tessdata/
```

- If you plan to fine-tune model from `xxxxx.traineddata`, download the **best** data files `https://github.com/tesseract-ocr/tessdata_best`
- unzip the file and move to relevant folder

```shell
unzip tessdata_best.zip
sudo mv tessdata_best /usr/share/tesseract-ocr/share/
```

- See [Segmentation fault when using integer models for LSTM training](https://github.com/tesseract-ocr/tesseract/issues/1573) for more informations
- Only the **float models** in tessdata_best can be used for **lstmtraining**
- Both tessdata and tessdata_fast have **integer models**

## Install Qt-box-editor (Optional) [link](https://github.com/chenghanc/Install-NVIDIA-Driver-CUDA-cuDNN-and-OpenCV/tree/tesseract3)

## Install jTessBoxEditor [link](https://github.com/chenghanc/Install-NVIDIA-Driver-CUDA-cuDNN-and-OpenCV/tree/tesseract3)

## Training Tesseract (Generate Training Images and Box Files)

Follow the instructions here [link](https://github.com/tesseract-ocr/tesstrain), [link](https://github.com/tesseract-ocr/tessdoc) and [link](https://github.com/livezingy/tesstrain-win)

<details><summary><b>CLICK ME</b> - Training Procedure</summary>

Before training your custom dataset, it is recommended to train [ocrd-testset.zip](https://github.com/tesseract-ocr/tesstrain/blob/main/ocrd-testset.zip) with sample ground truth first. This dataset consists of **line images** and **transcriptions**, line images have the extension `.tif`, transcriptions have the same name as the line images with the extension replaced by `.gt.txt` and must be single-line plain text. Download `tesstrain` by running the following command

```shell
git clone https://github.com/tesseract-ocr/tesstrain
```

Go to the folder `tesstrain` and extract `ocrd-testset.zip` to `./data/foo-ground-truth` and run `make training`. If the dataset can be trained normally, it means that the current training environment is OK and we can start to prepare custom dataset and perform training

**The output of successful training:**

```shell
Finished! Error rate = 1.134
lstmtraining \
--stop_training \
--continue_from data/foo/checkpoints/foo_checkpoint \
--traineddata data/foo/foo.traineddata \
--model_output data/foo.traineddata
Loaded file data/foo/checkpoints/foo_checkpoint, unpacking...
```

**Choose model name / Fine-tune / Ratio of training dataset:**

The default model name is `foo`

```shell
grep -nr MODEL_NAME .

...
./Makefile:19:MODEL_NAME = foo
...
```

We can give custom dataset a name when training model

```shell
make training MODEL_NAME=name_of_the_resulting_model

name_of_the_resulting_model=foo or hkid etc
```

The default model is trained from scratch, the `START_MODEL` in Makefile is assigned an empty string

```shell
grep -nr START_MODEL .

...
./Makefile:40:START_MODEL =
...
```

We can start fine-tuning from `eng.traineddata`

```shell
make training MODEL_NAME=name_of_the_resulting_model START_MODEL=eng TESSDATA=/usr/share/tesseract-ocr/share/tessdata_best MAX_ITERATIONS=10000
```

The ratio of training dataset is defined by the `RATIO_TRAIN` variable

```shell
grep -nr RATIO_TRAIN .

...
./Makefile:107:RATIO_TRAIN := 0.90
...
```

Run `make help` to see all the possible targets and variables


**How to train your custom dataset: Training Procedure**

- **Step 0:** Provide ground truth

- **Step 0:** Activate `jTessBoxEditor`

- **Step 1:** Generate

- **Step 10:** Check and test

```shell
tesseract --list-langs

tesseract 20211101311.jpg stdout -l eng
```


</details>



---

## References

1. [tesseract3](https://github.com/chenghanc/Install-NVIDIA-Driver-CUDA-cuDNN-and-OpenCV/tree/tesseract3)
2. [improve quality](http://coddingbuddy.com/article/51897036/how-can-i-improve-tesseract-results-quality)
