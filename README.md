# PAIP_2023
Tumor cellularity prediction in pancreatic cancer (supervised learning) and colon cancer (transfer learning). PAIP 2023 has been selected as a challenge of the IEEE International Symposium on Biomedical Imaging (ISBI 2023)
 
 ## Motivation:
 Tumor cellularity (TC) is used to compute the residual tumor burden in several organs. The TC is measured based on semantic cell segmentation, which accurately classifies and delineates individual cells. Essentially, tumor cellularity should be calculated by individual cell counting; however, manual counting is impossible, and human pathologists cannot avoid individual differences in diagnostic performance. Automatic image analysis is the ideal method for solving this problem, and it can efficiently reduce the workload of pathologists.
 
 ## Requirements:
 Platform: Paperspace
 
 Environment: Python 3
 
 Libraries: Tensorflow, Keras, Matplotlib, OpenCV, Scipy, Skimage, Numpy 

 ## Dataset:
#### 1. Number of challenge datasets

| Dataset | Pancreas | Colon | Total |
| :------------ |:------------:|:------------:| ------------: |
| Training | 50 | 03 | 53 |
| Validation | 10 | - | 10 |
| Test | 20 | 20 | 40 |
| Total | 80 | 23 | 103 |

#### 2. Data Characteristics
* Image patch files will be given in png file format (1024 X 1024 pixels)
* Image patch files were extracted from WSI scanned by Leica Aperio AT2 or GT450 stained by hematoxylin and eosin (H&E)
* Pancreas data: scanned at 20X magnification or 40X magnification
* Colon data: scanned at 40X magnification
* All personal labels in the images were removed in order to protect the privacy of patients

#### 3. Annotations:
* Two labels of the tumor cell nucleus and non-tumor cell nucleus segmentation will be provided for each image patch file in png file format (1024 X 1024 pixels). 
* Every cell respectively has a different number of distinguishing each cell.
* TC values between 0 to 100 (without decimal points) will be provided with MPP(Microns Per Pixel) in an excel file. 
* The labeled data were saved as uint 16.
* Labels will be provided for training datasets only.
* Organ information will be given in the name of the file.  

## Approach:
* <ins>Pre-processing</ins>: Initially, the individual tumorous cell and non-tumorous cell nuclei masks are combined to produce a unified image. Due to the unique pixel values of each nucleus, the image needs to be normalized.
* <ins>Segmentation using U-Net</ins>: For this particular problem, we have employed the U-Net architecture. Specifically, the encoder and decoder components of the U-Net were constructed using the pre-trained ResNet34 model, which we directly imported from the segmentation models library of Keras.
* <ins>Handling multichannel image data for segmentation</ins>: Specifically,  the resulting predicted image is of size (1024 x 1024 x 3), representing three color channels - red, green, and blue.  In order to facilitate the counting of different types of nuclei, we segregated tumorous nuclei to the red channel, non-tumorous nuclei to the green channel, and background to the blue channel.
* <ins>Cell counting using Watershed algorithm</ins>: To perform the task of counting tumorous and non-tumorous nuclei, we utilized the watershed algorithm, specifically, the marker-controlled watershed.
* <ins>Tumor cellularity calculation</ins>: The numerical count obtained from the previous step is utilized in the subsequent formula to calculate tumor cellularity (TC), which represents the percentage of tumor cells present in a given tissue sample.
 
       Tumor Cellularity(TC) =  number of tumorous nuclei / total number of nuclei
       
       
