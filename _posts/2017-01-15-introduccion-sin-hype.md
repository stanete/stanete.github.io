---
title: Introducción a machine learning pero sin hype
updated: 2017-01-15 11:00
---

> Aprendizaje supervisado & Clasificación

Vamos a saltarnos la parte de explicar qué es [Machine Learning](https://es.wikipedia.org/wiki/Aprendizaje_autom%C3%A1tico) y para qué sirve. Vamos a empezar directamente con cosas más prácticas.

PD: En próximos posts hablaremos de Deep Learning también.

## ¿Cómo se empieza?

**Algoritmos**. Necesitamos aprender cómo funcionan los algoritmos que ya existen (al menos algunos de ellos) y entender en qué situaciones es mejor utilizar uno u otro.

No vamos a programar esos algoritmos sino que vamos a usar python y varias librerías opensource para usarlos directamente y verlos en acción (sí que vamos a tratar de entender cómo funcionan).

A estos algoritmos también se les llama **modelos**. Un modelo sirve para representar de forma sencilla una realidad mucho más compleja. El problema es que eso nunca es exacto y siempre va a existir un **error**. Por eso, nuestro trabajo será maximizar la precisión con la que un modelo es capaz de clasificar, predecir o describir esa realidad.

Para empezar necesitamos tener instalado python y varios paquetes:

- [python 3.5 o 3.6](https://www.python.org/)
- [numpy](http://www.numpy.org/)
- [scipy](https://www.scipy.org/)
- [matplotlib](http://matplotlib.org/)
- [scikit-learn](http://scikit-learn.org/)

PD 1: Si los instalamos en este orden via *pip*, no deberíamos tener ningún problema.

PD 2: Yo utilizo [PyeCharm](https://www.jetbrains.com/pycharm/) y tengo todo dentro de un [virtualenv](https://virtualenv.pypa.io/en/stable/).

PD 3: Los ejemplos de código están hechos para que se puedan seguir con una consola de python en el orden que se muestran.

PD 4: Casi siempre debemos seguir el mismo proceso:

1. Recolectar y preparar datos
2. Escojer un modelo
3. Entrenar el modelo con los datos
4. Probar el modelo

<div class="divider"></div>

## Recolectar y preparar datos

Es improbable (no imposible) que nosotros desarrollemos un nuevo algoritmo que de mejores resultados que los que ya existen. Pero un algoritmo sin datos no sirve de nada. Un algoritmo puede dar resultados muy diferentes dependiendo de la estructura de los datos con los que lo alimentamos. Por eso gran parte de nuestro trabajo será recolectar y manipular datos. A esto se le llama *feature engineering*. Una feature o **característica** es cualquier medida de nuestros datos.

> Una buena característica es simple, independiente e informativa.

Para empezar vamos a usar el dataset de las flores Iris, un conjunto de datos clásico de los años treinta. Hoy en día, las diferentes especies de Iris se clasificarían por sus firmas genómicas pero en la década de 1930, el ADN no había sido identificado como el portador de la información genética, pero sí que se las podían clasificar por su morfología:

- Longitud del sépalo (sepal length (cm))
- Anchura del sépalo (sepal width (cm))
- Longitud de los pétalos (petal length (cm))
- Anchura de los pétalos (petal width (cm))

Lo bueno es que este dataset viene dentro de *scikit-learn*:

```python
from sklearn import datasets

iris = datasets.load_iris()
```

Para simplificarlo al máximo, vamos a trabajar solamente con las primeras dos características:

```python
features = iris.data[:, :2]

feature_names = iris.feature_names[:2]

print(feature_names)

# ['sepal length (cm)', 'sepal width (cm)']
```

Los datos de este dataset están etiquetados (labeled). Eso quiere decir que cada conjunto de características corresponde a una subespecie de Iris:

``` python
labels = iris.target

labels_names = iris.target_names

print(labels_names)

# ['setosa' 'versicolor' 'virginica']
```

Vamos a visualizar estos datos con *matplotlib*. Representamos cada subespecie por separado con un color difernte:

```python
from matplotlib import pyplot as plt

for l, c in zip(range(3), "brw"):
    plt.scatter(features[labels == l, 0],
                features[labels == l, 1],
                c=c)

plt.xlabel(feature_names[0])
plt.ylabel(feature_names[1])

plt.show()
```

![figure_1](../images/1_figure_1.png)

Visualizar los datos nos puede dar pistas sobre qué modelo utilizar. Por eso, **siempre tenemos que visualizar los datos**. El problema es que los humanos somos muy buenos visualizando datos en 2 e incluso en 3 dimensiones, pero no más. Para esto hemos elegido sólo dos características. En próximos posts hablaremos sobre cómo escoger las caracerísticas que más información aportan a la hora de visualizar los datos.

Por último, necesitamos **dividir** este dataset en datos para entrenar el modelo y datos para testearlo. Más adelante entenderemos por qué esto es extremadamente importante.

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

## Escojer un modelo

Dependiendo de la situación debemos usar un modelo u otro. Uno de los fallos de alguien que empieza en Machine Learning es pensar que las redes neuronales son la solución para todo, y eso no cierto.

El problema que estamos intentando resolver es de **aprendizaje supervisado**. Eso quiere decir que dados ejemplos etiquetados (labeled), podemos diseñar una regla que eventualmente se aplicará a otros ejemplos.

Concretamente, este es un problema de **clasificación**. Una vez que hayamos encontrado esa regla, podremos clasificar nuevas flores Iris según su subespecie. Para encontrar esa regla o serie de reglas debemos usar un modelo llamado **clasificador**.

Con *scikit-learn* tenemos disponibles varios clasificadores. Nosotros vamos a usar un **clasificador bayesiano ingenuo** o **Naïve Bayes**, uno de los clasificadores más utilizados por su simplicidad y rapidez. Quién era Bayes y por qué su clasificador es ingenuo lo veremos en próximos posts.

```python
from sklearn.naive_bayes import GaussianNB

clf = GaussianNB()
```

Vamos a alimentar al clasificador con los datos que hemos dividio:

```python
clf.fit(features_train, labels_train)
```

El clasificador ha construido unas **fronteras de decisión** o **superficies de decisión** para dividir el espacio vectorial en varios conjuntos, uno para cada subespecie de Iris. El clasificador clasificará todos los puntos de un lado de cada frontera de decisión como pertenecientes a una subespecie.

Vamos a visualizar las **fronteras de decisión** junto a los datos, aunque no nos vamos a preocupar por los detalles del código por ahora:

```python
import numpy as np

# Visualizar las fronteras de decisión.
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

# Visualizar también los datos.
for l, c in zip(range(3), "brw"):
    plt.scatter(features[labels == l, 0],
                features[labels == l, 1],
                c=c)
plt.xlabel(feature_names[0])
plt.ylabel(feature_names[1])

plt.xlim(xx.min(), xx.max())
plt.ylim(yy.min(), yy.max())
plt.show()
```

![figure_2](../images/1_figure_2.png)

Dependiendo de qué lado de cada frontera esté una nueva flor Iris, el clasificador le asignará una etiqueta u otra.

<div class="divider"></div>

## Poner el modelo a prueba

Ya hemos entrenado el clasificador pero no sabemos cómo de preciso es a la hora de clasificar nuevas flores Iris. Para averiguarlo vamos a testear el clasificador con los datos que hemos dividido antes. Vamos a hacer que nuestro clasificador determine las etiquetas de algunas flores de Iris y compararlo con sus etiquetas reales.

```python
from sklearn.metrics import accuracy_score

labels_predict = clf.predict(features_test)

accuracy = accuracy_score(labels_predict, labels_test)

print("Accuracy: %.2f" % (accuracy))

# Accuracy: 0.73
```

0.73 es mejor que la casualidad, pero no demasiado teniendo en cuenta que la precision se mide entre 0 y 1.

Vamos a intentar entrenar nuestro modelo con las 4 características y ver si la precisión del clasificador mejora:

```python
# Las 4 características.
features = iris.data

# Dividir datos.
features_train, features_test, \
    labels_train, labels_test = train_test_split(
        features, labels,
        train_size=0.8,
        test_size=0.2,
        random_state=0)

# Alimentar el clasificador con los datos de entrenamiento.
clf.fit(features_train, labels_train)

# Predecir etiquetas sobre los datos de testeo.
labels_predict = clf.predict(features_test)

# Calcular precisión.
accuracy = accuracy_score(labels_predict, labels_test)

print("Accuracy: %.2f" % (accuracy))

# Accuracy: 0.97
```

Wow! La precisión ha mejorado hasta 0.97. Como vemos, no todo depende del clasificador que usemos.

<div class="divider"></div>

## Para terminar

Machine Learning no es una caja mágica, sino matemáticas y código. Esto es sólo el principio y en próximos posts vamos a ver más algoritmos y más tipos de problemas.
