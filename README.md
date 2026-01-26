# Neural-Network-Based Prediction of Seismic Shakemaps from Simulated Ground Motions

## Summary
A convolutional neural network that predicts regional earthquake shakemap images from simplified rupture and site parameters, enabling faster inference than traditional physics-based methods.


## Repository Contents
```
├── Experiment 1 - Upsampling Models/
│ ├── CNN architectures incorporating upsampling layers and three loss functions
│ ├── Upsampling layers include Transposed Convolutions and Resize Convolutions
│ └── Loss functions are variations of pixel-wise intensity error (Global, Channel-wise, &   
│     structural similarity index combination)
│
├── Experiment 2 - No Upsampling Models/
│ ├── CNN architectures (6-layers) without upsampling layers and three loss functions
│ └── Ditto loss function comment in Experiment 1
│
├── Experiment 3 - Bias Improvements (Increase Layers and Filter Sizes) and Error Analysis/
│ ├── Deeper CNN architectures with increased layers (9 and 12 layers) and filter counts
│ └── Error analysis across input regimes to characterize model bias
│
├── Final_Model.ipynb
│ └── Final selected CNN model, training results, and evaluation
│
├── Shakemap_Data_Generator.ipynb
│ └──Dataset generator - simulates earthquakes and corresponding ground-truth shakemaps
```

## Technical Overview
Shakemap generation is framed as a scalar-to-image regression problem. Model features include relevant earthquake rupture and site soil parameters for all discrete points in the shaking region: earthquake magnitude, distance to rupture, site soil Vs30, longitude, latitude. Output is an RGB (0 to 1 pixel instensities) shakemap for a 24.05 km x 24.05 km area/region. The final model type is a 9-layer CNN with per-example input volume of 75x75x5 (shakemap is discretized into 75x75 grid with each point having 5 input parameters) and output volume of 75x75x3 (75x75 resolution RGB images).

Final loss function is a normalized channel-wise L2 loss on RGB pixel intensities...  
L = E [ (1/3) ∑_c ||Y_c − Ŷ_c||₂ / (||Y||₂ + ε) ]

Quantitative performance metrics are the "Mean Score" (MS) and "Stdv Score" (SS). These are aggregate metrics of the average and standard deviation of the per-example Mean Absolute Pixel Error (MAPE) and Structural Similarity Index (SSIM)...  
MS = 0.5[(1 - average(MAPE)) + average(SSIM)]   --->   Greater MS is better  
SS = 0.5*[stdv(MAPE) + stdv(SSIM)]   --->   Lesser SS is better


## Implementation (Code and Shakemaps for Disaster Response Planning)
(1) To run Shakemap_Data_Generator.ipynb, download the Vs30 Raster (U.S. Vs30 GeoData) from USGS...  
https://usgs.maps.arcgis.com/apps/webappviewer/index.html?id=8ac19bc334f747e486550f32837578e1  
(2) All kernel's and output cleared for all model files excpet Final_Model.ipynb  
(3) To use the produced shakemap image for risk-response planning, lower the image opacity and lay it over a map of the specified region (use shakemap center point latitude and longitude to anchor the placement)

(4) For Shakemap_Data_Generator.ipynb and model files, must update file path variables 


