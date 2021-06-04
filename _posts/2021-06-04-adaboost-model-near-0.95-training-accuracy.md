---
layout: post
title: "AdaBoost model near 0.95 training accuracy"
date: 2021-06-04 07:40:00 -0700
tags: classification, python, scikit-learn, AdaBoost, MNIST
published: true
---

Tuning the number of estimators, learning rate, and max depth for the base estimator tree has improved the model.  Now, I need to investigate whether or not that the model is over-fitting the test data.  New submission to the Kaggle competition is in the works.

![Accuracy results for n_estimators = 5](/assets/estimators_5.png)

![Accuracy results for n_estimators = 10](/assets/estimators_10.png)

![Accuracy results for n_estimators = 15](/assets/estimators_15.png)

![Accuracy results for n_estimators = 20](/assets/estimators_20.png)

![Accuracy results for n_estimators = 25](/assets/estimators_25.png)

![Accuracy results for n_estimators = 30](/assets/estimators_30.png)
