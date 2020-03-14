![Logo](http://47.116.58.103/ct-model/static/ct-imgs/logo.png)
# CT-Angel
---

## Table of Contents

* Background
* Data
* Model Development
* Usage
* Directory Structure
* About the author

---
## 1. Background
Computed tomography (CT) is the preferred imaging method for diagnosing 2019 novel coronavirus (COVID19) pneumonia. CT-Angel aimed to construct a system based on deep learning for detecting COVID-19 pneumonia on high resolution CT, relieve working pressure of radiologists and contribute to the control of the epidemic.

![Background](http://47.116.58.103/ct-model/static/ct-imgs/background.png)
## 2. Datasets
### 2.1 Datasets  source
* Hospital
   * From Renmin Hospital of Wuhan University (Wuhan, Hubei province, China). 
* Instruments
   * Included Optima CT680, Revolution CT and Bright Speed CT scanner (all GE Healthcare).
### 2.2 Datasets  amount
#### 2.2.1 Raw datasets
* 46,096 CT images from 106 patients
    * 25989 images from 51 COVID19 patients
    * 20107 images from 55 controls
#### 2.2.2 Selected datasets(after filtering those images without good pneumonia)
* 35355  CT images selected
    * 20886 images from 51 COVID19 patients
    * 14469 images from 55 controls
## 3. Model  development
### 3.1 Workflow  diagram 
![Workflow](http://47.116.58.103/ct-model/static/ct-imgs/workflow.png)
### 3.2 Model  training
#### 3.2.1 Training the model for extracting valid regions of CT images
   We first trained UNet++ to extract valid areas in CT images using 289 randomly selected CT images and tested it in other 600 randomly selected CT images. The training images were labelled with the smallest rectangle containing all valid areas by researchers. The model successfully extracted valid areas in 600 images in testing set with an accuracy of 100%.
#### 3.2.2 Training the model for detecting suspicious lesions in CT images
   For detecting suspicious lesions on CT scans, 691 images of COVID-19 pneumonia infection lesions labelled by radiologists and 300 images randomly selected from patients of non-COVID-19 pneumonia were used. Taking the raw CT scan images as input with a resolution of 512×512, and the labelled map from the expert as output, UNet++ was used to train in Keras in an image-to-image manner. The suspicious region was predicted under a confidence cutoff value of 0.50, and a prediction box pixel of over 25. 
### 3.3 Model  testing
#### 3.3.1 Testing datasets
| Methods | COVID-19 Patients amount | COVID-19 images amount | Other Patients amount | Other images amount |
| :----: | :----: | :----: | :----: | :----: |
| Retrospective testing | 11 | 4382 | 31 | 9369 |
| Prospective testing | 16 | / | 11 | / |
#### 3.3.2 Testing results
| Methods | Sensitivity | Specificity | Accuracy | PPV | NPV |
| :----: | :----: | :----: | :----: | :----: | :----: |
| Retrospective testing | |||||
| Per patient | 100% | 93.55% |95.24%|84.62%|100%|
| Per image | 94.34% | 99.16% |98.85%|88.37%|99.61%|
| Prospective testing(Per patient) | 100% | 81.82% |92.59%|88.89%|100%|
 ```
 Ps: PPV: positive prediction value,NPV: negative prediction value.
 ```
#### 3.4 Representative images of model testing
![Results](http://47.116.58.103/ct-model/static/ct-imgs/results.png)

* A. Computed tomography (CT) images of COVID19 pneumonia. The predictions between the artificial intelligence model and radiologists were consistent. Green boxes, labels from radiologists; red boxes, labels from the model. 
* B. CT images of the control. 
## 4. Usage
### 4.1 Environmental  dependence
#### 4.1.1 Computer reference configuration
| Item | Configuration |
| :----: | :----: |
| Operating system | Microsoft Windows 10 64-bit |
| Cpu | Intel Corei7-8700 3.2GHz |
| RAM | >=16G |
| Hard disk | 256G solid state hard disk + 1T mechanical hard disk|
| Graphics card | NVIDIA GeForce RTX 2080 (8G video memory) |
#### 4.1.2 Dependent tools
| Tools | Version |
| :----: | :----: |
| Python | 3.5+ |
| Tensorflow | 1.13.1 |
| Keras | 2.2.4 |
| Unet++ | / |
| OpenCV | 3.4.2 |
| CUDA | 10.1 |
| CUDnn | 10 |
### 4.2 Instructions for use
#### 4.2.1 Ready to adapt the hardware
#### 4.2.2 Download and install the required toolkit
   * [Python 3.5+](https://www.anaconda.com/download/) https://www.anaconda.com/download/
   ```
      It is recommended to download the anaconda3 integrated environment directly, which is more convenient.
   ```
   * Tensorflow 1.13.1
     We chose to install TensorFlow-GPU on Anaconda:
     1. Create an environment dedicated to TensorFlow：
        ```
        conda create -n tf python=3.6
        ```
     2. Then enter this environment. In order to ensure that there are no problems with the installation, it is recommended to update the pip and setuptool tools：
        ```
        activate tf #Enter the tensorflow environment
        deactivate #Exit the tensorflow environment
        python -m pip install --upgrade pip 
        pip install -–upgrade setuptools
        ```
     3. Then you can install TensorFlow:
        ```
        pip install tensorflow-gpu==1.13.1
        ```
     4. After the installation is complete, enter the python interpreter and import TensorFlow. If the import is successful, the installation is successful.
   * Keras 2.2.4 
   ```
     pip install Keras==2.2.4
   ```
   * Unet++
   ```
     We have prepared a package for Unet ++ for you, which is the folder named unet_pp.
   ```
   * OpenCV 3.4.2
   ```
     pip install opencv-python==3.4.2
   ```
   * [CUDA 10.1](https://developer.nvidia.com/cuda-downloads) https://developer.nvidia.com/cuda-downloads
   * [CUDnn 10](https://developer.nvidia.com/rdp/cudnn-download) https://developer.nvidia.com/rdp/cudnn-download
   ```
     Need to register, install after registration.After decompression, place the three files in the corresponding directories of cuda.
   ```
#### 4.2.3 Download our trained model
We open sourced two models we trained: the model for extracting valid regions of CT images and the model for detecting suspicious lesions in CT images. The download links are provided below:
```
link：https://pan.baidu.com/s/1zwDztKY0zSvhtBE48J-2vw 
extraction code：zide
```
#### 4.2.4 Download our project
#### 4.2.5 Run our project locally
A jupyter file named ```ct_angel_train``` is provided in our project. You can follow the prompts of the code comments in the jupyter file to place the training pictures and corresponding mask pictures in the corresponding path and folder, and then run a jupyter file 
to start our project and get the prediction result of your own CT images.
## 5. Directory Structure
```
├── Readme.md                            // ct-angel introduction&help
└── ct-angel-train                       // ct-angel use package
│   ├── data                             // ct-angel data
│      └── model_data                    // ct-angel model data
│          └── ct-angel_weights.txt      // model download link file
│      └── train_images                  // ct-angel model train_images
│          └── DJ20200112A0189(243).jpg  // train_images sample 
│      └── train_labels                  // ct-angel model train_labels 
│          └── DJ20200112A0189(243).jpg  // train_labels sample
│      └── val_images                    // ct-angel model val_images 
│          └── DJ20200112A0189(243).jpg  // val_images sample 
│      └── val_labels                    // ct-angel model val_labels 
│          └── DJ20200112A0189(243).jpg  // val_labels sample 
│      └── test_images                   // ct-angel model test_images
│          └── DJ20200112A0189(243).jpg  // test_images sample
│      └── test_labels                   // ct-angel model test_labels
│          └── DJ20200112A0189(243).jpg  // test_labels sample
│   ├── sample                           // ct-angel prediction sample
│      └── sample01.jpg                  // sample image
│   ├── unet_pp                          // unet++ package
│   └── ct_angel_train.ipynb             // ct-angel-train-code
```
## 6. About the author
* Author
  * Renmin Hospital of Wuhan University & Wuhan EndoAngel Medical Technology Co., Ltd.
* E-mail
  * shan.hu@ai-endoangel.com
  * kuo.zhang@ai-endoangel.com
  * jinzhu.liu@ai-endoangel.com
