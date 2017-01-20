---
title: Clasificador bayesiano ingenuo
updated: 2017-01-20 12:00
comments: true
mailchimp: true
---

> Teorema de Bayes, independencia condicional, vantajas & desventajas

Este es un mini post de **Machine Learning pero sin hype**, una serie sobre aprendizaje automático (o de máquinas) en español.

Hemos visto en el [primer post](introduccion-machine-learning-sin-hype) que un clasificador puede, a partir de una serie de características, predecir una etiqueta (o clase).

Los clasficadores bayesianos ingenuos o **Naïve Bayes** son una familia de algoritmos de **clasificación probabilística** basados en el [**teorema de Bayes**](https://es.wikipedia.org/wiki/Teorema_de_Bayes) y en la asumción de [**independencia condicional**](https://es.wikipedia.org/wiki/Independencia_condicional).

Todos comparten el principio de que **todas las características utilizadas son independientes**, es decir, asumen que ninguna característica tiene ningún tipo de relación con cualquier otra.

**En la vida real las característica no son siempre independientes**, sino que tienen relación entre ellas. Precisamente por esta asumción tan fuerte se dice que el clasificador es **ingenuo** 🤔.

<div class="divider"></div>

## Un ejemplo

Una fruta puede ser considerada una manzana si es roja, redonda y tiene unos 10cm de diámetro. Un clasificador bayesiano ingenuo considera que cada una de estas características contribuye de manera independiente a la probabilidad de que esta fruta sea una manzana, sin importar la relación que puede haber entre estas características o si alguna de estas características no está presente.

<div class="divider"></div>

## Ventajas y desventajas

Ventajas:

- Es sencillo de comprender y programar.
- Es fácil de entrenar, incluso con datasets pequeños.
- Es muy rápido!
- No es sensible a características irrelevantes.

Desventajas:

- Asume que todas las característica son independientes, lo cual no siempre es verdad.

<div class="divider"></div>

## Vamos a terminar esto

Aunque se basan en una idea relativamente sencilla, esta familia de clasificadores puede superar en muchas ocasiones a algoritmos mucho más complejos 😌. En el próximo post vamos a usar un **clasificador bayesiano ingenuo** y vamos a ver que **es increíblemente efectivo en la deteción de spam y clasificación de documentos**.

Te puedes suscribir y recibir un email cada vez que haya un nuevo post.
