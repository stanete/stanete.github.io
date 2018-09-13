---
title: Machine learning! But without the hype...
updated: 2018-13-09 11:35
comments: true
mailchimp: true
image: /images/introduccion-machine-learning-sin-hype.png
---

> First steps, workflow, supervised learning & classification

This is the first post from the series **Machine learning! But without the hype...**. It's 2018 so by this point everybody has at least some understandig of what machine learning is and what it is useful for. That's why we are going to skip the introducction and we are going to jump right into it.

Don't worry. We will talk about Deep Learning too 😏.

## Where do we start 🤔

**Algorithms**. We need to get comfortable with how the algorithms that are already out there work. At least with some of them and understand when is better to use one or other depending on the problem we need to solve.

We are not going to program those algorithms ourselves. We are going to use python and different opensource libraries to apply those algorithms directly and observe them in action. We are going to try to understand the magic underneath.

These algorithms are also called **models**. **A model represents a compelx reality in a simpler way**. The problem is that any representation wont't be perfect and there will always going to be an **error**. Our job will be to maximize the precision with which a model is able to classify, predict or describe that reality 🤔.

The easiest and most common way to get started is with an iPython notebook. That way we can start playing with some machine learning libraries and models right away. 

The fastest way to get an iPython notebook up and runnign is using a [docker](https://www.docker.com) 🐳 container. After installing docker just run the following command in your working repository:

```
docker run -p 8888:8888 -v $(pwd):/src stanete/scikit-learn
```

This will download a docker image with everything you need and will run a container. It will show you a console message containing a URL that you will have to copy and paste on your browser. Just like magic ✨.

<div class="divider"></div>

## The workflow 🚜

We will follow the same workflow on almost every machine learning problem. We will talk about it with more detail in future posts. But for now here is a simplified version of it:

1. Gathering and preparing the data
2. Choosing a model
3. Training the model
4. Testing the model

<div class="divider"></div>

## Gathering and preparing the data 🏗

It is improbable (difficult but not imposible) for us to develop new types of models. However a model without data isn't useful. In most cases, a model won't be able to learn from completely arbitrary data. The data needs to be presented to the model in a way that will make the model’s job easier. A model can achieve very different results depending on the structure of the data it's fed with. That's why a big part of our job will be to gather and manipulate that data to suit the needs of a certain model. That is called *feature engineering*.

To get started we are going to use the dataset of the Iris flowers. This is a classic dataset from the 30s that has features about 3 subspecies of Iris flowers:

![iris_flowers](../images/iris_flowers.png)


Nowadays the different subspeciees of Iris flowers will be classified by their genomic blueprint but in the 30s the DNA was not discovered yet. So we would classify them by their morfology:

- Sepal length (cm)
- Sepal width (cm)
- Petal length (cm)
- Petal width (cm)

### Loading the data

This dataset comes packed in with *scikit-learn*:

```python
from sklearn import datasets

iris = datasets.load_iris()
```

To simplify this to the maximum we are going to work only with the first two features:

```python
features = iris.data[:, :2]

feature_names = iris.feature_names[:2]

print(feature_names)

# ['sepal length (cm)', 'sepal width (cm)']
```

The data in this dataset is labeled. That means that any feature set corresponts to an Iris subspecies:

``` python
labels = iris.target

labels_names = iris.target_names

print(labels_names)

# ['setosa' 'versicolor' 'virginica']
```

### Visualizing the data 📊

We are going to use *matplotlib* to visualize the data. We represent each subspecies with a different color:

```python
from matplotlib import pyplot as plt

for l, c in zip(range(3), "brw"):
    plt.scatter(features[labels == l, 0],
                features[labels == l, 1],
                c=c)

# Naming axes x and y.
plt.xlabel(feature_names[0])
plt.ylabel(feature_names[1])

# Show the graph.
plt.show()
```

![figure_1](../images/1_figure_1.png)

Visualizing the data can give us hints about what model to use. That's why we should **always visualize the data**. We humans are very good to visualize data in 2 dimensions and even in 3 dimensions. But it's imposible to visualize more than that. That's why we have chosen only 2 features.

### Training and testing data 

Lastly, we need to **separate** this dataset into a training set and a testing set. We will talk in future posts about the reason behind it and why this is extremly important.

```python
from sklearn.model_selection import train_test_split

features_train, features_test, \
    labels_train, labels_test = train_test_split(
        features, labels,
        train_size=0.8,
        test_size=0.2,
        random_state=0)
```

<div class="divider"></div>

## Choosing a model

We are trying to solve a **classification** problem and we are going to do it with a **supervised learning** model. A **classifier**. That means we have available a set of already classified or **labeld** data and the model will infer some rules that will apply to classify new data.

*scikit-learn* has available different classifiers. We are going to use the **Naïve Bayes** classifier which is by far the most popular one mostly for its simplicity.

```python
from sklearn.naive_bayes import GaussianNB

clf = GaussianNB()
```

Let's **feed the classifier** with the training data:

```python
clf.fit(features_train, labels_train)
```

## What happened undeneath 🤔

The classifier has built some **decision borders** or **surfice borders** to divide the vectorial space in different sets, one for every Iris subspecies. Let's visualize those **decision borders** along with the data:

```python
import numpy as np

# Visualize the decision borders.
h = .02
x_min = features[:, 0].min() - 1
x_max = features[:, 0].max() + 1
y_min = features[:, 1].min() - 1
y_max = features[:, 1].max() + 1
xx, yy = np.meshgrid(np.arange(x_min, x_max, h),
                     np.arange(y_min, y_max, h))

Z = clf.predict(np.c_[xx.ravel(), yy.ravel()])

Z = Z.reshape(xx.shape)
plt.contourf(xx, yy, Z, cmap=plt.cm.coolwarm, alpha=0.8)

# Visualize the data.
for l, c in zip(range(3), "brw"):
    plt.scatter(features[labels == l, 0],
                features[labels == l, 1],
                c=c)

#  Naming axes x and y.
plt.xlabel(feature_names[0])
plt.ylabel(feature_names[1])

# Define the graphs limits.
plt.xlim(xx.min(), xx.max())
plt.ylim(yy.min(), yy.max())

# Show the graph.
plt.show()
```

![figure_2](../images/1_figure_2.png)

Depending on which side of the border is going to fall a new Iris flower, the classifier will assign a label or other.

<div class="divider"></div>

## Testing the model 👮‍

We have already trained the model but we don't know how **accurate** it is when classifying new Iris flowers. To find out we are going to test the classfier with the testing data. 

Let's make the classifier to determine (or predict) the labels of some Iris flowers and compare them with their real labels:

```python
from sklearn.metrics import accuracy_score

# Predict labels for the testing data.
labels_predict = clf.predict(features_test)

# Calculate precision comparing
# predicte labels with real labels.
accuracy = accuracy_score(labels_predict, labels_test)

print("Accuracy: %.2f" % (accuracy))

# Accuracy: 0.73
```

0.73 is better than chance, but not too much. 

Let's try to train the model with the 4 features and see if the accuracy of the classifier improves:

```python
# The 4 features.
features = iris.data

# Divide the data.
features_train, features_test, \
    labels_train, labels_test = train_test_split(
        features, labels,
        train_size=0.8,
        test_size=0.2,
        random_state=0)

# Feed the classifier with training data.
clf.fit(features_train, labels_train)

# Predict labels for the testing data.
labels_predict = clf.predict(features_test)

# Calculate precision comparing
# predicte labels with real labels.
accuracy = accuracy_score(labels_predict, labels_test)

print("Accuracy: %.2f" % (accuracy))

# Accuracy: 0.97
```

Wow 👏! The accuracy has improved a lot. As you see not everything depends on the model we use.

<div class="divider"></div>

## Wrapping up

Machine Learning is not a magic box. It's calculus, algebra, statistics and code. This is only the beginning and in next posts we are going to explore more types of models and more types of problems to solve.

<div class="divider"></div>

You can subscribe and receive an email any time there is a new post. Any feedback is welcomed 🤗!!
