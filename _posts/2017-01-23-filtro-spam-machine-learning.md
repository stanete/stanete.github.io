---
title: Data processing and feature extraction
updated: 2017-01-23 08:45
comments: true
mailchimp: true
image: /images/filtro-spam-machine-learning.png
redirect_from: "/clasificador-bayesiano-ingenuo"
---

> Extracting features, tekenization, naïve Bayes classiffier & documents classification

This is the second post from the series **Machine learning! But without the hype...**. In this post we will focus on the process of preparing the data for a model and have a grasp of how tedious it can be.

We are going to create a spam filter which is the **Hello World** of the classification of docuemnts in Machine Learning. In this post we are going to create a simpe spam filter from real emails labeled as *spam* or *ham* (emails that are not spam).

Again you will need to have [docker](https://www.docker.com) 🐳 installed and in your working directory run:

```
docker run -p 8888:8888 -v $(pwd):/src stanete/scikit-learn
```

El único paquete con el que no hemos trabajado todavía es **pandas**. De momento sólo necesitamos saber que es una librería de python opensource para análisis de datos.

The workflow we are going to follow is the same as in [first post](machine-learning-without-hype):

1. Gathering and preparing the data
2. Choosing a model
3. Training the model
4. Testing the model

<div class="divider"></div>

## Gathering and preparing the data 🏗

We will have to learn to love this step because in real life is going to be the one in which we spend the most time on.

### Downloading the data

We are going to use the public dataset [Enron-Spam](http://www.aueb.gr/users/ion/data/enron-spam/) which we are going to download and place in a folder named *data*. This dataset contains preprocessed emails divided in folders *spam* and *ham*.

Let's define the labels (or classes) of this dataset and the routs of the directories that contain the emails that we have just dowloaded:


```python
# Labels.
HAM = 'ham'
SPAM = 'spam'

# Datasources with the routes and folders
# that contain the corresponding emails.
SOURCES = [
    ('data/enron1/spam', SPAM),
    ('data/enron1/ham', HAM),
    ('data/enron2/spam', SPAM),
    ('data/enron2/ham', HAM),
    ('data/enron3/spam', SPAM),
    ('data/enron3/ham', HAM)
]
```

### Loading the data

Let's load the data in a [DataFrame](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html), a data structure in which we can think as a dictionary built upon **numpy**. The DataFrame will contain the bodies of the emails in one column and the corresponding label in the other column.

```python
from pandas import DataFrame

# Define two columns. One for the email text body
# and other one for the label.
data = DataFrame({'text': [], 'label': []})
```

Let's create a function that, given the route of the directory, it will read every file in that directory and will return the route of the file together with the email text. We are going to use the route of the file as index for every DataFrame entry.

```python
import os

def read_files(path):
    # Obtaining files names.
    file_names = os.listdir(path)

    # Loop over the file names.
    for file_name in file_names:

        # The route of the file.
        file_path = os.path.join(path, file_name)

        lines = []

        # Open the file.
        f = open(file_path, encoding="latin-1")
        for line in f:
            # Add every line of the email.
            lines.append(line)

        # Close the file.
        f.close()

        # Join the lines.
        text = '\n'.join(lines)

        # Return the route of the file and the text.
        yield file_path, text
```

We are also going to define a function that will build and return a DataFrame from the bodies of the emails obtain with *read_files*. This function will also assign to each email its correspondat label. Every entry in the DataFrame is going to be indexed by the route of the file that contains the email.

```python
def build_data_frame(path, label):
    rows = []
    index = []

    # Loop over the files given a path.
    for file_path, text in read_files(path):
        # Add the text of the file with its 
        # correspondat label.
        rows.append({'text': text, 'label': label})

        # Use the frame of the file as index.
        index.append(file_path)

    # Create a new DataFrame
    # from the rows and indexes. 
    data_frame = DataFrame(rows, index=index)

    # Finally return the DataFrame.
    return data_frame
```

We are going to use those two functions*read_files* y *build_data_frame* to build a DataFrame from the emails we have:

```python
# Loop over the paths of the
# directories that contain the email:
for path, label in SOURCES:
    # Add each DataFrame to a dictionary 
    # with its correspondant label
    data = data.append(build_data_frame(path, label))
```

To ensure that the data are not ordered in a uniform way, we are going to shuffle them:

```python
import numpy

data = data.reindex(numpy.random.permutation(data.index))
```

### Extract features

Before we can train a model to classify the emails we need some **features**. In documents classification, the frequency with which a word appears seems to be a good feature.

Let's create a table with all the words mentioned in the **corpus** and their frequency in each email label (spam or ham).

|      |    Linux   |   today    |   Viagra   |    Free    |
|:----:|:----------:|:----------:|:----------:|:----------:|
| ham  | 619        | 67         | 0          | 50         |
| spam | 3          | 432        | 291        | 534        |

This process is called **tokenización** because **transforms a collection of text documents to a matrix of tokens count**. Using *scikit-learn* this is easy:

```python
from sklearn.feature_extraction.text import CountVectorizer

count_vectorizer = CountVectorizer()

# The emails we have in the DataFrame.
email_bodies = data['text'].values

# Learn the vocabulary of the corpus
# and extract the tokens.
features = count_vectorizer.fit_transform(email_bodies)

# The labels (spam or ham).
labels = data['label'].values
```

### Training and testing data

Lastly we need to **separate** this dataset in training and testing sets so we can train the model and then test it with different data.

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

The probelm we are trying to solve is a **supervised learning**, and more concretely a **clasification**.

We are going to use a naïve bayes classifier, one of the fastest and simplest probabilistic classifiers. It's based on the [Bayes Theorem](https:google.es) and on the asumption of [**contitional independecy**]. This classifier assumes that **all features are used independend of one another**, that is, no feature has any relation with one another. **In real life the features are not completely independent**. Precisely because of this strong assumption we say that the classifier is **naïve**.

In this case, we assume that the probability with which a word appears does not influence the probablity of other words appearing.

```python
from sklearn.naive_bayes import MultinomialNB

clf = MultinomialNB()
```

<div class="divider"></div>

## Training the model with data

Let's **train the classifier** with the data we have separated:

```python
clf.fit(features_train, labels_train)
```

This is the **learning process**. From labeld examples, the bayes classifier **learns** the probabilities that each token appearence contributes to that label (or class). Given a new email, the classifier will use those probabilities to predict its label (or class), in this case *spam* o *ham*.

<div class="divider"></div>

## Probar el modelo

We have already trained the classifier but we don't know how **accurate** it is when classifing new emails. To figure it out we are going to test the classifier with the testing data set we have separated before. 

Let's make our classifier determine (or predict) the labels of some emails and compare those predicted labels with the real ones.

```python
from sklearn.metrics import accuracy_score

# PRedict labels for the testing emails.
labels_predict = clf.predict(features_test)

# Calculate the accuracy comparing
# the predicted labels and the real ones.
accuracy = accuracy_score(labels_predict, labels_test)

print("Accuracy: %.2f" % (accuracy))

# Accuracy: 0.99
```

Wow 👏! An accuracy of 99% 😳

We can also test our filter with new emails:

```python
# Testing emails.
test_emails = ['Free Viagra!',
               "I'm going to attend the meeting tomorrow."]

# Tokenization.
test_features = count_vectorizer.transform(test_emails)

# Predict labels.
labels_predict = clf.predict(example_features)

print("Predictions: %s" % labels_predict)

# Predictions: ['spam' 'ham']
```

The spam filter works perfectly 🎉🎉🎉!!

<div class="divider"></div>

## Wrapping up

The naive bayes classifiers work very well to classify documents. the real life spam filters are of course much more complex but ours works very great as well.

<div class="divider"></div>

You can subscribe and receive an email any time there is a new post. Any feedback is welcomed 🤗!!
