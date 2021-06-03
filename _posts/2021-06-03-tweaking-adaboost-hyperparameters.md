---
layout: post
title: "Tweaking AdaBoost hyperparameters"
date: 2021-06-03 08:34:00 -0700
tags: classification, python, scikit-learn, AdaBoost, MNIST
published: true
---

I have to admit, yesterday I had already given up on using AdaBoost on the MNIST data set.  But I had only been adjusting its n_estimators and learning_rate hyperparameters.  So, I decided to try using a large number of estimators (200 - 1000), tweaking the learning rate (0.2,0.25,0.3,0.35), and changing the classification tree max depth for the base estimator (1,2,3).  

```python

from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

X_train, X_val, y_train, y_val = train_test_split(X, y)

n_trees = [200, 400, 600, 800, 1000]
depths = [1,2,3]
rates = [0.2,0.25,0.3,0.35]

for n_estimators in n_trees:
    ada_clf.n_estimators = n_estimators;
    
    for depth in depths:
        ada_clf.base_estimator =  tree(random_state=619,max_depth=depth)

        for learning_rate in rates:
            ada_clf.learning_rate = learning_rate
            
            ada_clf.fit(X_train, y_train)
            y_pred = ada_clf.predict(X_val)
            accuracy = accuracy_score(y_val, y_pred)
            print(n_estimators,depth,learning_rate,accuracy)

```

The results are promising: this model blew past the training accuracy scores attained by the CART classification tree I used a few days ago on this data set.  **The best model has an accuracy of 0.896** (rounded to the nearest thousandth); using 800 estimators, a max depth of 3, and a 0.20 learning rate.

<!-- | n_estimators  |max_depth  |learning_rate  | accuracy
| :---          | :---      | :---          | :---
| 200           | 3         | 0.20          | 0.890
| 200           | 3         | 0.25          | 0.890
| 400           | 3         | 0.20          | 0.893
| 600           | 3         | 0.20          | 0.895
| 600           | 3         | 0.25          | 0.890
| 800           | 3         | 0.20          | 0.896
| 800           | 3         | 0.25          | 0.894
| 1000          | 3         | 0.20          | 0.895
{: .display} -->

