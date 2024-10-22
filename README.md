# Solafune_Aerosol_Optical_Depth_Estimation

### Competition hosted on [Solafune](https://solafune.com/competitions/ca6ee401-eba9-4f7d-95e6-d1b378a17200?menu=about&tab=overview)
### Overview
Aerosol Optical Depth (AOD) is a critical parameter for understanding air quality and climate change, as it quantifies the amount of aerosols present in the atmosphere. The Sentinel-2 mission, part of the European Space Agency's Copernicus program, provides high-resolution multispectral imagery, offering a valuable resource for remote sensing applications. In this competition, participants are challenged to estimate AOD using Sentinel-2 data, leveraging its spectral bands and spatial resolution to improve accuracy.
This endeavor aims to enhance the precision of AOD measurements, facilitating better environmental monitoring and decision-making. Accurate AOD estimation will have significant implications for public health, weather forecasting, and climate research. The ultimate goal is to advance the methodologies for AOD estimation, contributing to global efforts in environmental protection and climate change mitigation. Participants' contributions will pave the way for more accurate and reliable AOD datasets, benefiting researchers, policymakers, and society at large.

### Data Overview
#### Sentinel-2 images

The Sentinel-2 image contains information on 12 bands and includes the following bands 'B1', 'B2', 'B3', 'B4', 'B5', 'B6', 'B7', 'B8', 'B8A', 'B9', 'B10', 'B11', 'B12'

For more information, please visit [this site](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S2_SR_HARMONIZED#bands)

The images have been processed to mask clouds for Sentinel-2 images from 2016/1/1 to 2024/5/1, and each image that sentinel-2 captured is based on the availability of the satellite images at the time combined with the AERONET dataset.

### Competition Policy
#### cf. @solafune(https://solafune.com) Use for any purpose other than participation in the competition or commercial use is prohibited. If you would like to use them for any of the above purposes, please contact us.

#### Evaluation metric is Pearson R Coefficient

### My Approach
### Exploratory Data Analysis
  * Target class distribution analysis.
  * Visualize a sample image with 12 bands by target class.
  * Histogram analysis for each band by target class
  * Visualize Sentinel-2 popular RGB composites by target class
  * Visualize remote sensing indices by target class
    * NDVI - Normalized Difference Vegetation Index
    * NDWI - Normalized Difference Water Index
    * FMI - Ferrous Mineral Index
    * MSI - Moisture Stress Index
    * BSI - Bare Soil Index
    * NBR - Normalized Burn Ratio
  * Basic image-level information analysis
    * Entropy
    * Contrast
    * Blur
  * Image duplication analysis
    * Find duplication images using the perceptual hashing technique.
      
The notebook for exploratory data analysis is available on Kaggle.[![Open in Kaggle](https://img.shields.io/static/v1?label=&message=Open%20in%20Kaggle&labelColor=grey&color=blue&logo=kaggle)](https://www.kaggle.com/code/hari141v/solafune-finding-mining-sites-eda)

### Model

### Data Preparation
 * Removed low entropy images.
 * Train data split into 5 stratified kfold
 * Data Augmentation
   * Image augmentation using albumentation
     * RandomRotate90
     * HorizontalFlip
     * VerticalFlip
     * SafeRotate
     * CoarseDropout
 * Used WeightedRandomSampler in the training dataset data loader with a batch size of 4.
### Model
 * Utilized all 12 bands for training.
 * Trained the maxvit_tiny_tf_512 model on the five-fold training data with the listed augmentations. Ten epochs were used for training the five-fold dataset, and early stopping was implemented to control overfitting by monitoring the validation log loss.
 * Model parameters
   * Loss: CrossEntropyLoss with weight
   * Optimizer: AdamW
   * Learning rate: 1e-4
   * Weight decay: 1e-2
   * LR scheduler: CosineAnnealingLR
 * Post-training, select the model with the lowest validation loss and found an optimal threshold for classification.
 * Predicted the test data using the five-fold model, applying test-time augmentation to ensure confident predictions.
 * Steps for test image prediction:
   * For each image, obtained results from the five-fold model, applying test-time augmentation 5 times. Thus, the final number of predicted probabilities for a single image is 25.
   * Calculated the mean of the 25 predictions and then applied an optimal threshold to determine the final result class.
 * Tracked the model's performance using [WANDB](https://wandb.ai/hari141v/Solafune_Finding_Mining_Sites_maxvit_tiny_tf_512_in1k_12ch_removed_low_entr_img/overview?nw=nwuserhari141v).
 * [Five fold training results](https://github.com/hariprasath-v/Solafune_Finding_Mining_Sites/blob/main/maxvit_tiny_tf_512_in1k_5_fold_eval_results_ch12_low_entr_img_removed.csv)

# Final Competiton Rank: 37/169 | Score: 0.914611005693
