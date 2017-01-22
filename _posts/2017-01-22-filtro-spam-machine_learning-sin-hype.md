---
title: Filtro de spam con Machine Learning pero sin hype
updated: 2017-01-23 23:32
comments: true
mailchimp: true
---

> Extraer características, tokenización & clasificación de documentos

Este es el segundo post de **Machine Learning pero sin hype**, una serie sobre aprendizaje automático (o de máquinas) en español. En este post vamos a construir un filtro de spam bastante preciso a partir de emails reales etiquetados como *spam* o *ham* (emails que no son spam).

Necesitamos tener instalado [python 3.5 o 3.6](https://www.python.org/) y varios paquetes:

- [numpy](http://www.numpy.org/)
- [scipy](https://www.scipy.org/)
- [pandas](http://pandas.pydata.org/)
- [scikit-learn](http://scikit-learn.org/)

Si los instalamos en este orden via *pip*, no deberíamos tener ningún problema. Yo utilizo [PyCharm](https://www.jetbrains.com/pycharm/) y tengo todo dentro de un [virtualenv](https://virtualenv.pypa.io/en/stable/).

El único paquete con el que no hemos trabajado todavía es **pandas**. De momento sólo necesitamos saber que es una librería de python opensource para análisis de datos.

El proceso que seguirémos será el mismo que en el [primer post](introduccion-machine-learning-sin-hype):

1. Recolectar y preparar datos
2. Escoger un modelo
3. Entrenar el modelo con los datos
4. Probar el modelo

<div class="divider"></div>

## Recolectar y preparar datos

Vamos a tener que enamorarnos de este paso porque es en el que vamos a pasar gran parte del tiempo a la hora de trabajar en Machine Learning.

### Descargar datos

Vamos a trabajar con el dataset público [Enron-Spam](http://www.aueb.gr/users/ion/data/enron-spam/) que vamos a descargar y poner en una carpeta llamada *data*. Este conjunto de datos contiene emails preprocesados divididos en carpetas *spam* y *ham*.

Primero tenemos que definir las etiquetas (o clases) de este dataset y las rutas de las carpetas que contienen los emails que hemos descargado.

```python
# Etiquetas.
HAM = 'ham'
SPAM = 'spam'

# Fuentes de datos con las rutas de
# las carpetas que contienen los emails
# y las etiquetas correspondientes.
SOURCES = [
    ('data/enron1/spam', SPAM),
    ('data/enron1/ham', HAM),
    ('data/enron2/spam', SPAM),
    ('data/enron2/ham', HAM),
    ('data/enron3/spam', SPAM),
    ('data/enron3/ham', HAM)
]
```

### Cargar datos

Vamos a cargar los datos en un [DataFrame](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html), una estructura de datos en la que podemos pensar como un diccionario construido sobre **numpy**. Este DataFrame contendrá los cuerpos de los emails en una columna y la etiqueta correspondiente en otra columna.

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

Antes de que podamos entrenar un algoritmo para clasificar los emails, necesitamos unas características (features) para hacerlo. En la clasificación de documentos, la frecuencia con la que aparece cada palabra es una buena característica.

Vamos a crear una tabla con todas las palabras mencionadas en el corpus(colleción de emails que tenemos) y su frecuencia en cada clase de email (spam o ham):

|      |    Linux   |   today    |   Viagra   |    Free    |
|:----:|:----------:|:----------:|:----------:|:----------:|
| ham  | 619        | 67         | 0          | 50         |
| spam | 3          | 432        | 291        | 534        |

Este proceso también se llama **tokenización** porque transformamos una colección de documentos de texto a una matriz de recuento de tokens. Con *scikit-learn* esto es muy fácil:


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

Vamos a usar un [**clasificador bayesiano ingenuo**](clasificador-bayesiano-ingenuo), uno de los clasificadores más utilizados por su simplicidad y rapidez. Vamos a ver que es increíblemente efectivo en la detección de spam y clasificación de documentos.

```python
from sklearn.naive_bayes import MultinomialNB

clf = MultinomialNB()
```

Vamos a **entrenar el clasificador** con los datos que hemos dividio:

```python
clf.fit(features_train, labels_train)
```

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

Te puedes suscribir y recibir un email cada vez que haya un nuevo post.
