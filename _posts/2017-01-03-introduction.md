---
title: Introducción a Machine Learning
updated: 2015-09-06 15:56
---

> Sin nada de hype.

[https://es.wikipedia.org/wiki/Aprendizaje_autom%C3%A1tico]

Vamos a saltarme la parte de explicar qué es Machine Learning y para qué sirve. Vamos a empezar directamente con cosas más prácticas.

PD: También vamos a tratar Deep Learning dentro de poco tiempo.

## ¿Cómo se empieza?

Algoritmos. Necesitamos aprender cómo funcionan los algoritmos que ya existen (al menos algunos de ellos) y entender en qué situaciones es mejor utilizar uno u otro.

No vamos a programar esos algoritmos sino que vamos a usar python y varias librerías opensource para usarlos directamente y verlos en acción (sí que vamos a tratar de entender cómo funcionan por debajo).

A estos algoritmos también se les llama modelos. Un modelo sirve para representar de forma sencilla una realidad mucho más compleja. El problema es que eso nunca es exacto y siempre va a existir un error. Por eso, nuestro trabajo será maximizar la precisión con la que un modelo es capaz de clasificar, predecir o describir esa realidad.

Casi siempre tenemos que seguir el mismo proceso:

1. Recolectar y preparar datos
2. Escojer un modelo
3. Entrenar el modelo con los datos
4. Probar el modelo

Para empezar neceistamos tener instalado:

- python 3.5 o 3.6 [https://www.python.org/]
- scikit-learn [http://scikit-learn.org/]
- matplotlib [http://matplotlib.org/]


## Recolectar y preparar datos

> Prepare the data. Separate into train & test features & labels. Plot the data.

Necesitamos datos. Muchos.

Es improbable (no imposible) que nosotros desarrollemos un nuevo algoritmo que de mejores resultados que los que ya existen. Pero un algoritmo sin datos no sirve de nada. Un algoritmo puede dar resultados muy diferentes dependiendo de la estructura de los datos con los que lo alimentamos. Por eso gran parte de nuestro trabajo será recolectar y manipular datos. A eso se le llama *feature engineering*. Una feature o características es cualquier medida de nuestros datos.

> Una buena característica es simple, independiente e informativa.

Para empezar vamos a usar el dataset de las flores Iris, un conjunto de datos clásico de los años treinta. Hoy en día, las diferentes especies de Iris se clasificarían por sus firmas genómicas pero en la década de 1930, el ADN no había sido identificado como el portador de la información genética, pero sí que se las podían clasificar por su morfología:

- Longitud del sépalo (sepal length (cm))
- Anchura del sépalo (sepal width (cm))
- Longitud de los pétalos (petal length (cm))
- Anchura de los pétalos (petal width (cm))

Lo bueno es que este dataset viene dentro de scikit-learn:

```python

>>> from sklearn import datasets

>>> iris = datasets.load_iris()

```

Para simplificarlo al máximo, vamos a trabajar solamente con las primeras dos características:

```python

>>> data = iris.data[:, :2]

```

Cada set de características en este dataset corresponde a una subespecie de Iris:

``` python

>>> target_names = iris.target_names

array(['setosa', 'versicolor', 'virginica'],
      dtype='<U10')

>>> target = iris.target

```


Vamos a visualizar estos datos rápidamente:

```python

>>>  from matplotlib import pyplot as plt

>>>

```


TODO: PLOT THE FREAKING DATA WITH MATPLOTLIB

Lo siguiente será dividir nuestros datos en datos para entrenar nuestro modelo y datos para poner a prueba nuestro modelo. Más tarde veremos por qué esto es extremadamente importante y cómo jugar con estos datos pero de momento confía en mí.

```python
from sklearn.model_selection import train_test_split

features_train, features_test, labels_train, labels_test = train_test_split(iris.data, iris.target,
                                                                            test_size=0.4,
                                                                            random_state=0)
```

## Aprendizaje supervisado [https://es.wikipedia.org/wiki/Aprendizaje_supervisado] - Clasificación

Este es un problema de aprendizaje supervisado o clasificación; Dado ejemplos etiquetados (labeled), podemos diseñar una regla que eventualmente se aplicará a otros ejemplos. Esta es la misma configuración que se utiliza para la clasificación de spam; Teniendo en cuenta los ejemplos de spam y ham (no spam de correo electrónico) que el usuario dio al sistema, podemos determinar si un nuevo mensaje entrante es spam o no.

TODO: BETTER EXPLAIN WHAT A CLASSIFIER DOES

## Clasificador Bayesiano ingenuo

[https://es.wikipedia.org/wiki/Red_bayesiana]

```python

>>> from sklearn.naive_bayes import GaussianNB

>>> clf = GaussianNB()
```

Entrenar nuestro modelo:

```python

clf.fit(features_train, labels_train)

```

TODO: PLOT THE FREAKING MODEL

## Poner el modelo a prueba

Tenemos que calcular la precisión de nuestro modelo. En concreto, cómo de preciso es nuestro modelo a la hora de clasificar una flor Iris dados sus datos morfológicos.


```python
from sklearn.metrics import accuracy_score

labels_predict = clf.predict(features_test)

accuracy = accuracy_score(labels_predict, labels_test)

print("Accuracy: ", accuracy)

```

TODO: EXPLAIN EVERYTHING
