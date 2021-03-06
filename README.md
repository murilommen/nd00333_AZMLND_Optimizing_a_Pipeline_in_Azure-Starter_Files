# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Summary
This problem involves a dataset containing information on the customer and y, the target variable, which indicates wether the customer subscribed to a fixed term deposit or not.
The best performing model was a gradient boosting-based model, using the LightGBM framework.

## Scikit-learn Pipeline
The architecture for the Sk-learn approach was indeed rather simple. First, it reads the data from the specified endpoint (an URL in this specific case), then it will clean the data, one hot encode it and split them into training data and target column. Next, split into train and test and finally trains the Logistic Regression model and log the accuracy from the run. 
In order to get the highest possible accuracy, an hyperparameter tuning was performed with the aid of HyperDrive and varied the "C" parameter - which corresponds to the inverse of regularization strenght - from the values uniformly distributed between `0.1` and `100`. The best performing model with this architecture was the one with C=16, that got `90.6%` Accuracy.

By choosing the uniform sampler with the Random Sampling method, we can guarantee that a majority of the grid of hyperparameters will be covered, with lower computational efforts.
The early stopping method used (Bandit) will also prevent unnecessary computation by taking the metrics that are too far from the best run by a slack factor and stop the run there.

## AutoML
The AutoML framework generated the LightGBM with a feature scaling method called Maximum Absolute Scaler, with `91.7%` Accuracy. 

## Pipeline comparison

The AutoML framework made it possible with very low effort to find a better performing algorithm than what the HyperDrive achieved before. It then showed us a way to choose a good framework, and also a better scaling methodology. The gain in Accuracy was close to `1%`.
All in all, the LightGBM is a Gradient Boosting algorithm, which is a technique that puts together sets of "weak" prediction models, normally decision trees. It is a fast and more general approach, and when more complexity and enthropy are identified on the dataset, it shows to be more robust. The logistic regression on the other hand gives us with very low computational effort a way to explain the variable importances and how they affect the end result with a good split between the classes.

## Future work
In the near future, it would be a good idea to use the scaling method and the LightGBM framework and do a more complete hyperparameter search with HyperDrive. That could potentially lead to even better metrics for us to take the model to production, which would also be effortless with Azure ML.
