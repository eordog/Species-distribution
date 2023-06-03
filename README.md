# Species-distribution

This repository contains models that predict species density given natural and anthropogenic factors. This project was created for the Erdos Institute Data Science May 2023 Boot Camp by Erika Ordog, Kristen Scheckelhoff, and Rohan Sarkar, with project mentor Patrick Vallely.


Background
----------

We are interested in exploring the effects of environmental
and human factors on animal species distribution. Some an-
imal species are known to adapt and thrive in the face of
expanding urbanization, but with this increased human ac-
tivity also comes competition for resources. Identifying key
factors which influence species density in urban areas can
lead to policy recommendations for more sustainable urban
growth.

Data
-------

We used the mammal population densities dataset [1] com-
piled by Tucker et al. for a recent article [2] published in the
journal Ecography. The authors concluded that mammal
populations densities were higher in urban areas, but also
noted that this may be due to related factors, e.g. humans
choosing to build in areas with abundant natural resources.
The dataset includes the Landsat Normalized Difference Veg-
etation Index (NDVI), a possible environmental predictor of
species density.

Exploratory data analysis
--------------------------



Regression Models
---------------------

### Baseline Model ###

The baseline model uses the mean species density (measured in individuals per square kilometer). 

### Linear regression###


### k-nearest neighbors###

### XGBoost regressor ###

The XGBoost regressor model is a gradient-boosting ensemble model that starts with a decision tree and continually improves the model by traning on the errors of the predictions. 

### Random forest regressor ###

Random forests average over many decision trees on bootstrapped samples of the data to improve accuracy and prevent overfitting.
With hyperparameter tuning, the best random forest regressor had 200 estimators and no maximum depth or maximum number of features.

### AdaBoost regressor ###

This AdaBoost regressor starts with a decision tree and continually improves the model by giving weights to the data based on the prediction error.
Hyperparameter tuning gave a learning rate of 0.012, 7 estimators, and the decision tree baseline estimator had a max depth of 20.



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
