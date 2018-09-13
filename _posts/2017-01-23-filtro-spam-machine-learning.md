---
title: Create a spam filter using Machine Learning
updated: 2017-01-23 08:45
comments: true
mailchimp: true
image: /images/filtro-spam-machine-learning.png
redirect_from: "/clasificador-bayesiano-ingenuo"
---

> Extracting features, tekenization, naïve Bayes classiffier & documents classification

This is the second post from the series **Machine learning! But without the hype...**.

Creating a spam filter is the **Hello World** of the classification of docuemnts in MAchine Learning. In this post we are going to create a simpe spam filter from real emails labeled as *spam* or *ham* (emails that are not spam).

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

# Definir dos columnas. Una para el texto de los
# emails y otra para la etiqueta correspondiente.
data = DataFrame({'text': [], 'label': []})
```

Vamos a crear una función que, dada la ruta de una carpeta, va a leer todos los ficheros de esa carpeta y va a devolver la ruta del fichero junto con el texto del email. Vamos a utilizar la ruta del fichero como index para cada entrada del DataFrame.

```python
import os

def read_files(path):
    # Obtener los nombres de todos los ficheros.
    file_names = os.listdir(path)

    # Recorrer los ficheros.
    for file_name in file_names:

        # La ruta del fichero.
        file_path = os.path.join(path, file_name)

        lines = []

        # Abrir el fichero.
        f = open(file_path, encoding="latin-1")
        for line in f:
            # Añadir cada línea del email.
            lines.append(line)

        # Cerrar el fichero.
        f.close()

        # Unir las líneas.
        text = '\n'.join(lines)

        # Devolver la ruta del fichero y el texto.
        yield file_path, text
```

También vamos a definir una función que va a construir y devolver un DataFrame a partir de los cuerpos de los emails obtenidos con *read_files*. Esta función también va a asignarle a cada email con su etiqueta (label) correspondiente. Cada entrada del DataFrame va a ser indexada por la ruta del archivo de cada email.

```python
def build_data_frame(path, label):
    rows = []
    index = []

    # Recorrer todos los ficheros dada un ruta.
    for file_path, text in read_files(path):
        # Añadir el texto del fichero junto
        # con su etiqueta correspondiente.
        rows.append({'text': text, 'label': label})

        # Utilizar la ruta del fichero como index.
        index.append(file_path)

    # Crear un nuevo DataFrame
    # a partir de las filas y los indexes.
    data_frame = DataFrame(rows, index=index)

    # Devolver el DataFrame.
    return data_frame
```

Vamos a usar estas dos funciones, *read_files* y *build_data_frame*, para construir un DataFrame a partir de los emails que tenemos:

```python
# Recorrer las rutas de las
# carpetas que contienen los emails:
for path, label in SOURCES:
    # Añadir cada DataFrame que devuelve
    # la función build_data_frame dada la
    # ruta de la carpeta y su etiqueta correspondiente.
    data = data.append(build_data_frame(path, label))
```

Para asegurarnos de que los datos no están ordenados de forma uniforme, vamos a mezclarlos:

```python
import numpy

data = data.reindex(numpy.random.permutation(data.index))
```

### Extraer características

Antes de que podamos entrenar un algoritmo para clasificar los emails, necesitamos unas **características** (features). En la clasificación de documentos, la frecuencia con la que aparece cada palabra es una buena característica.

Vamos a crear una tabla con todas las palabras mencionadas en el **corpus**(colleción de emails que tenemos) y su frecuencia en cada clase de email (spam o ham):

|      |    Linux   |   today    |   Viagra   |    Free    |
|:----:|:----------:|:----------:|:----------:|:----------:|
| ham  | 619        | 67         | 0          | 50         |
| spam | 3          | 432        | 291        | 534        |

Este proceso se llama **tokenización** porque **transformamos una colección de documentos de texto a una matriz de recuento de tokens**. Con *scikit-learn* esto es muy fácil:

```python
from sklearn.feature_extraction.text import CountVectorizer

count_vectorizer = CountVectorizer()

# Los emails que tenemos en el DataFrame.
email_bodies = data['text'].values

# Aprender el vocabulario del corpus
# y extraer el recuento de tokens.
features = count_vectorizer.fit_transform(email_bodies)

# Las etiquetas (spam o ham).
labels = data['label'].values
```

### Datos de entrenamiento y datos de testeo

Por último, necesitamos **dividir** este dataset en datos para entrenar el modelo y datos para testearlo.

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

## Escoger un modelo

El problema que estamos intentando resolver es de **aprendizaje supervisado**, concretamente, **clasificación**.

Vamos a usar un **clasificador bayesiano ingenuo**, uno de los clasificadores probabilísticos más sencillos y rápidos. Está basado en el [**teorema de Bayes**](https://es.wikipedia.org/wiki/Teorema_de_Bayes) y en la asunción de [**independencia condicional**](https://es.wikipedia.org/wiki/Independencia_condicional). Este clasificador asume que **todas las características utilizadas son independientes**, es decir, que ninguna característica tiene ningún tipo de relación con otra. **En la vida real las característica no son siempre independientes**, sino que tienen relación entre ellas. Precisamente por esta asunción tan fuerte se dice que el clasificador es **ingenuo** 🤔.

En este caso, asumimos que la probabilidad de que aparezca una palabra no influye en la probabilidad de que aparezca cualquier otra.

```python
from sklearn.naive_bayes import MultinomialNB

clf = MultinomialNB()
```

<div class="divider"></div>

## Entrenar el modelo con los datos

Vamos a **entrenar el clasificador** con los datos que hemos dividio:

```python
clf.fit(features_train, labels_train)
```

Este es el **proceso de aprendizaje**. A partir de ejemplos etiquetados, el clasificador bayesiano **aprende** las probabilidades que cada token aporta a esa etiqueta (o clase). Dado un nuevo email, el clasificador usará esas probabilidades para predecir su etiqueta (o clase), en este caso *spam* o *ham*.

<div class="divider"></div>

## Probar el modelo

Ya hemos entrenado el clasificador pero no sabemos cómo de **preciso** es a la hora de clasificar nuevos emails. Para averiguarlo vamos a testear el clasificador con los datos que hemos dividido antes. Vamos a hacer que nuestro clasificador determine (o prediga) las etiquetas de algunas emails y compararlas con las etiquetas reales.

```python
from sklearn.metrics import accuracy_score

# Predecir etiquetas para los emails de testeo.
labels_predict = clf.predict(features_test)

# Calcular la precisión comparando
# las etiquetas predecidas y las etiquedas reales.
accuracy = accuracy_score(labels_predict, labels_test)

print("Accuracy: %.2f" % (accuracy))

# Accuracy: 0.99
```

Wow! Una precisión del 99% 😳

También podemos probar nuestro filtro de spam con nuevos emails:

```python
# Emails de prueba.
test_emails = ['Free Viagra!',
               "I'm going to attend the meeting tomorrow."]

# Tokenización.
test_features = count_vectorizer.transform(test_emails)

# Predecir etiquetas.
labels_predict = clf.predict(example_features)

print("Predictions: %s" % labels_predict)

# Predictions: ['spam' 'ham']
```

El filtro de spam funciona a la perfección 🎉🎉🎉!!

<div class="divider"></div>

## Vamos a terminar esto

Los clasificadores bayesianos ingenuos o Naïve Bayes funcionan muy bien para clasificar documentos. Y aunque en la vida real los filtros de spam son mucho más complejos, el nuestro funciona también bastante bien.

<div class="divider"></div>

Te puedes suscribir y recibir un email cada vez que haya un nuevo post.
