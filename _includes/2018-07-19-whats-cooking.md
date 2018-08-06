---
layout: post
title: "Kaggle: What's Cooking"
date: 2018-07-19T10:20:00Z
categories: Kaggle
---
<br>
Okay so welcome to our second blog entry on our solution to another [Kaggle](https://www.kaggle.com/) competition. This time we're looking at a compeition called [What's Cooking](https://www.kaggle.com/c/whats-cooking-kernels-only), this one is pretty interesting well for me at least because it actually is in the realm of some NLP (Natural Language Processing). Well some of the aspects of it can be applied here, so some of the things that I came across while doing my stuff for GSoC (Google Summer of Code) is actually kind of helpful. So essentially our approach looked at using count vectorizer, tfidf (term frequency inverse document frequency) and looking t using different classification algorithms (best being Linear Support Vector Multiclassification).

## Imports
So first we go about importing our different modules, as usual we have our usual pandas and numpy. However our main focus will be on using sklearn (scikit-learn) and importing things such as CountVectorizer and LinearSVC.

{% highlight python linenos %}
import pandas as pd
import numpy as np
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import accuracy_score
from sklearn.model_selection import train_test_split
from time import time
import matplotlib.pyplot as plt
from sklearn import tree
from sklearn.feature_extraction.text import CountVectorizer
import json
import matplotlib.pyplot as plt
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.feature_extraction.text import TfidfTransformer
from sklearn.feature_selection import chi2
from sklearn.model_selection import cross_val_score
from sklearn.linear_model import LogisticRegression
from sklearn.svm import LinearSVC
from sklearn.ensemble import RandomForestClassifier
import matplotlib.pyplot as plt
%matplotlib inline
{% endhighlight %}

## Opening Files
We then go about opening the "train.json" file and the "test.json" file. The data in these files are store as dictionaries. 

{% highlight python linenos %}
with open("train.json") as f:
    datastore = json.loads(f.read())
f.close()
with open("test.json") as f:
    test = json.loads(f.read())
f.close()
print(datastore[0])
print("\nNumber of recipes: ",len(datastore),"\n")
print(datastore[0]['ingredients'])
print(type(datastore))
print(type(datastore[0]))
length = len(datastore)
length1 = len(test)
max = 0
pos = 0
{% endhighlight %}


Resources:
[Coursera to python](https://github.com/arturomp/coursera-machine-learning-in-python/tree/master/mlclass-ex4-004/mlclass-ex4)

[Andrew Ng Machine Learning](https://www.coursera.org/learn/machine-learning)


Files Used:<br>
Jupyter Notebook - [digit_recognizer_blog_code](https://github.com/kmn5409/Digit-Recognizer/blob/master/digit_recognizer_blog_code.ipynb)







