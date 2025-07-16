# Project Title: Design of Alloys for gaining high strength to weight ratio
Overview:

This repository contains machine learning models developed for the "Design of Alloys with High Strength-to-Weight Ratio" project . The models aim to predict key properties of Aluminum (Al) alloys, specifically minimum density and maximum stress, to facilitate the design of new alloys with improved strength-to-weight ratios.

Features:

  Property Prediction: Machine learning models (Support Vector Regression - SVR) for predicting maximum stress and minimum density of Al alloys.
  
  Composition-Based Featurization: Utilizes the CBFV (Composition-Based Feature Vectors) library to generate relevant features from alloy chemical formulas.
  
  Data Preprocessing: Includes steps for data splitting (90% train, 10% test), feature scaling using StandardScaler, and handling of elemental properties.
  
  Robust Model Training: Employs GridSearchCV with GroupKFold cross-validation for hyperparameter tuning and robust model selection. GroupKFold ensures that samples with the same alloy composition are kept together during cross-validation, preventing data leakage.
  
  Uncertainty Quantification: Implements a bootstrapping method to estimate the mean and standard deviation of predictions, providing a measure of uncertainty for each prediction.
  
  Expected Improvement Calculation: Includes functionality to calculate Expected Improvement (EI), an acquisition function used in Bayesian optimization to identify promising new alloy compositions for further investigation.


