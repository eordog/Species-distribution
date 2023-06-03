# Species density

This repository contains models that predict species density given natural and anthropogenic factors. This project was created for the Erdos Institute Data Science May 2023 Boot Camp by Erika Ordog, Kristen Scheckelhoff, and Rohan Sarkar, with project mentor Patrick Vallely.


Background
----------

We are interested in exploring the effects of environmental and human factors on animal species distribution. Some animal species are known to adapt and thrive in the face of expanding urbanization, but with this increased human activity also comes competition for resources. Identifying key
factors which influence species density in urban areas can lead to policy recommendations for more sustainable urban growth.

Data
-------

We used the mammal population densities dataset [^1] compiled by Tucker et al. for a recent article [^2] published in the journal Ecography. The authors concluded that mammal populations densities were higher in urban areas, but also noted that this may be due to related factors, e.g. humans choosing to build in areas with abundant natural resources. The dataset includes the Landsat Normalized Difference Vegetation Index (NDVI), a possible environmental predictor of species density.

The features available in the dataset are: 
* Natural predictors
  * Normalized Difference Vegetation Index (NDVI)
  * Mass (g)
* Anthropogenic predictors
  * Human Footprint Index
  * Night time lights
  * Human population density
  * Crop land
  * Pasture
  * Accessibility

Exploratory data analysis
--------------------------

After some initial exploration, we selected a subset of 6 species with sufficiently many data points and similar habitats: Axis axis (chital deer), Loxodonta africana (African elephant), Panthera pardus (leopard), Panthera tigris (tiger), Sus scrofa (wild boar), and Syncerus caffer (African buffalo),
for a total of 1057 data points. Paired plots across the available features did not immediately reveal any strong trends, so we tested a variety of models.

![eda](https://github.com/eordog/Species-distribution/assets/97986688/8548f4a8-fc80-4041-9693-f68fd85b9e7c)


Regression Models
---------------------

### Baseline Model ###

The baseline model uses the mean species density (measured in individuals per square kilometer). 

### Linear regression ###

We fit linear regression models for species density using
NDVI, Human Footprint, and Species Richness (all at the
1-degree level of granularity) as the predictive features, as
well as log transforms of both the independent and depen-
dent variables, and a quadratic regression. None of these
models were significantly stronger than the baseline

### k-nearest neighbors ###

The k-nearest neighbors regressor predicts density by averaging the k closest data points, or neighbors. With hyperparamter tuning, the best model used k = 8 neighbors and used weighted averaging based on distance. 

![rmse_knn](https://github.com/eordog/Species-distribution/assets/97986688/acd5e0bf-6c8d-44f2-a97d-ee1c18041756)


### XGBoost regressor ###

The XGBoost regressor model is a gradient-boosting ensemble model that starts with a decision tree and continually improves the model by traning on the errors of the predictions. After hyperparameter tuning, the best XGBoost model had a learning rate of 0.5, max depth of 6, 500 estimators, and colsample_bytree of 0.7.

### Random forest regressor ###

Random forests average over many decision trees on bootstrapped samples of the data to improve accuracy and prevent overfitting.
With hyperparameter tuning, the best random forest regressor had 500 estimators, no max depth, and maximum features equal to the square root of the total number of features.


Results
---------------

The cross-validation and test set errors for the regression models are below. 
The best model was the random forest regressor, with a test set RMSE of 5.30.

| Model       | Cross-validation RMSE (Ind/km^2) | Test RMSE (Ind/km^2)    |
| :---        |    :----:        |      :---: |
|  Baseline model    |       8.50      |      7.23    |
|  Linear regression   | 7.95            | 6.18   |
|  XGBoost     |              5.49            |   6.22   |
|  k-nearest neighbors |      4.68         |   6.00    |
|  Random forest    |             4.83           |    5.30     |    

Feature importance
--------------------------

Our best model, random forest regression, determines feature importance by evaluating the impact of each feature on the predictive error of the model. 
In order of importance, the top five features impacting species density are:
1. Cropland
2. Night lights
3. Human population density
4. NDVI
5. Human footprint

[^1]: Marlee A. Tucker, Luca Santini, Chris Carbone, and Thomas Mueller. Mammal population densities at a global scale
are higher in human-modified areas. Dryad, Dataset, 2020. https://doi.org/10.5061/dryad.m63xsj40d.
[^2]: Marlee A. Tucker, Luca Santini, Chris Carbone, and Thomas Mueller. Mammal population densities at a global scale
are higher in human-modified areas. Ecography, 44(1):1–13, 2021.
