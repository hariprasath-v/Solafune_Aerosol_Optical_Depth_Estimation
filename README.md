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

### Model-1 

### Data Preparation
#### Data was split into training and validation sets randomly.
#### Band-wise pixels were chosen, and embeddings were created using the timm efficientnet_b0.ra_in1k model.
#### An XGBoost regressor model was created for 13 bands and evaluated on the validation dataset using the Pearson correlation coefficient score.

#### Band-wise validation results,

| Band   | Validation_score |
|--------|------------------|
| band_7 | 77               |
| band_6 | 75               |
| band_8 | 73               |
| band_2 | 71               |
| band_1 | 69               |
| band_9 | 69               |
| band_5 | 67               |
| band_3 | 66               |
| band_4 | 63               |
| band_12| 56               |
| band_10| 52               |
| band_13| 49               |
| band_11| 7                |

### Model-2 

### Data Preparation
#### Data was split into training and validation sets randomly.
#### An XGBoost regressor model was created for the following popular RGB composites and evaluated on the validation dataset using the Pearson correlation coefficient score.
- Natural Color
- False Color Infrared
- False Color Urban
- Agriculture
- Atmospheric Penetration
- Healthy Vegetation
- Land/Water
- Natural Colors with Atmospheric Removal
- Shortwave Infrared
- Vegetation Analysis
  
#### RGB composites validation results,

| RGB_Composites                             | Validation_score |
|--------------------------------------------|------------------|
| Natural Colors                             | 68               |
| False Color Infrared                       | 74               |
| False Color Urban                          | 66               |
| Agriculture                                | 67               |
| Atmospheric Penetration                    | 63               |
| Healthy Vegetation                         | 74               |
| Land Water                                 | 68               |
| Natural Colors with Atmospheric Removal    | 73               |
| Shortwave Infrared                         | 81               |
| Vegetation Analysis                        | 75               |


 
