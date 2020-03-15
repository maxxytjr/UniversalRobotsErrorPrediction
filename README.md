# Prediction of Errors during Operation of UR Cobots

# Background
The [Universal Robots'](https://www.universal-robots.com/) 6-axis collaborative robots is one of the most preferred choices for end users in helping them achieve their automation needs.

When deployed in production, errors (although rare) on the robot might occur and cause it to pause its operation. This might be due to internal faults or environmental factors.

Hence it will be useful if we could look at the robot's telemetry data (eg. joint current, temperature, voltage etc) and look for potential correlations between them and occurrences of operational errors. In doing so, we could also use these data to predict if an error is going to occur, or the behavior of various metrics using LSTM models.

# Method
Robot data can be extracted at a desired frequency via TCP/IP. In this case, a Python script was used to extract joint positions, velocities, currents, voltages, temperature and robot safety states at a frequency of 125Hz (1 data point every 8 milliseconds) over a period of 1 hour.  To learn more about the RTDE interface, click [here](https://www.universal-robots.com/how-tos-and-faqs/how-to/ur-how-tos/real-time-data-exchange-rtde-guide-22229/). 

Some data cleaning and feature engineering (created new feature representing joint accelerations) were done, before being used to train **Random Forest** and **Gradient Boosting** ensemble classifiers for predicting the occurrence of error states. Model fitting and tuning was done in a **10-fold cross validation** fashion. 

Model evaluation was based on training performance (**CV score**) and performance on test sets using the **AUROC** metric. The best model, along with its best hyperparameter combination, was selected.

# Results
## Confusion matrix
The confusion matrix tells us how many true positives, true negatives, false positives and false negatives there are in the predicted results. This gives us inituition as to how good the model is at differentiating between a positive (error) and negative (no error) class label.

## AUROC
The area under the ROC curve provides us the model's classification performance, based on the **True positive rate (TPR)** and **False positive rate (FPR)** metrics. The closer it is to 1, the better.

![ROC Curve](https://github.com/maxxytjr/UniversalRobotsErrorPrediction/blob/master/auroc.JPG)






