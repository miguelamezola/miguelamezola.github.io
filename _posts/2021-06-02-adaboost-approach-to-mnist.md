---
layout: post
title: "AdaBoost approach to MNIST"
date: 2021-06-02 15:02:00 -0700
tags: classification, python, scikit-learn, AdaBoost, MNIST
published: true
---

Three days ago, I trained a CART classification tree model on the MNIST data set.  I submitted its predictions to the Digit Recognizer competition on Kaggle, which uses this data set.  The submission received an accuracy score of 0.86346.

So, I was expecting for this AdaBoost model that I'm training today to perform better; its base algorithm is also a classification tree.  Surprisingly, it hasn't received an accuracy score greater than 0.81 on the training set.  I think this indicates that tree-based algorithms do not perform well on this data set.

![AdaBoost Notebook Screenshot](/assets/kaggle-adaboost-notebook-20210602.png)

