---
title: Mejora las revisiones de código sin ser tan pro
updated: 2020-05-30 08:00
comments: true
mailchimp: true
layout: post
image: /images/code_reviews.png
excerpt: En algún momento, en un equipo de Ingeniería de Producto tendrás que abordar problemas relacionados con las revisiones de código.
---

Puedes leer este post en inglés [aquí](/improve-code-reviews).

En algún momento, en un equipo de _Ingeniería de Producto_ tendrás que abordar problemas relacionados con las revisiones de código. En algunos equipos, el código se lleva a master sólamente mirándolo por encima porque las revisiones son meramente superficiales. En otros, el código se queda en el limbo durante mucho tiempo porque las revisiones son demasiado duras. En otros, las _Pull Requests_ se abandonan por completo porque nadie revisa el código en su día a día.

Algunos equipos más maduros, como Netlify, llegan a [soluciones bastante creativas](https://www.netlify.com/blog/2020/03/05/feedback-ladders-how-we-encode-code-reviews-at-netlify/), como adoptar una metáfora para dar feedback sobre las _Pull Requests_. Otros, como Squarespace, crean [guías](https://engineering.squarespace.com/blog/2019/code-review-culture-part-1) bastante [extensas](https://engineering.squarespace.com/blog/2019/code-review-culture-part-2) y Google tiene una [guía inlcuso más extensa](https://google.github.io/eng-practices/review/reviewer/standard.html). No tiene que ser todo tan profesional. Puede ser algo más simple. Quiero compartir mi enfoque para los equipos con los que he trabajado. De hecho, tenemos esto escrito en una página de Confluence.

![](/images/code_reviews.png)

Como regla general, una _Pull Request_ debe ser revisada por al menos un miembro de otro equipo. No evita los silos de conocimiento, pero ayuda a tener una idea de lo que está sucediendo en otros equipos. Para eso, necesitamos tener algunas reglas comunes sobre la creación y la revisión de _Pull Requests_, para que la experiencia sea lo más uniforme posible.

## Crear

La responsabilidad de las personas que crean una _Pull Request_ es hacer que el código sea lo más fácil de entender posible. Para facilitar el trabajo del revisor, el creador debe:

#### Dar suficiente contexto

Usa una [plantilla para las Pull Requests](https://help.github.com/en/github/building-a-strong-community/creating-a-pull-request-template-for-your-repository) que responda las siguiente preguntas:

- ¿Qué hace esta PR?
- ¿Por qué queremos hacer esto?
- ¿Qué cambios de alto nivel has hecho en el código?
- ¿Qué otra información debe tener en cuenta el revisor?

Proporciona información adicional como capturas de pantalla de UI o un GIF, un link al ticket de JIRA, etc.

#### Mantener las PRs de un tamaño manejable

Más de 300 cambios es una bandera roja. Más de 10 archivos es una bandera roja. Hay excepciones como cambiar el nombre de un paquete o mover archivos.

![](/images/pull_requests.png)

#### No escribir código que no se use en la propia PR

A menos que el código esté destinado a ser una API pública consumida por otros. Es cierto que esto es controvertido, pero si el código que llevamos a producción no se usa, ¿cómo podemos asegurarnos de que estamos agregando valor?

## Revisar

La responsabilidad de los revisores es asegurarse de que aceptan el código como suyo y lo cuidarán como si fuera escrito por ellos mismos. Diferentes Ingenieros mirarán diferentes aspectos del código, pero para garantizar una experiencia fluida para todos, el revisor debe:

#### Comprar un ticket para al menos los 4 autobuses de revisión de código

A primera hora de la mañana, antes de comer, después de comer y a última hora de la tarde. Tener al menos cuatro veces al día donde se revisa el código, ayuda bastante. Asegura que las PRs no se quedan demasiado tiempo sin que nadie las mire. También evita los recordatorios personales en Slack. Puedes usar [Trailer](http://ptsochantaris.github.io/trailer/) con GitHub para acelerar tu flujo de revisión, pero no es obligatorio.

![](/images/bus_stop.png)

#### Asegurarse de que la PR cumpla lo mínimo para ser aceptada

No seas demasiado blando con la revisión del código. Recuerda que estás aceptando el código como tuyo. Tampoco seas demasiado duro. Diferentes Ingenieros tienen diferentes formas de resolver un problema. Para que un código se acepte, solo tiene que cumplir con lo siguiente:

- Hace lo que se supone que tiene que hacer
- Ha sido testeado y los tests pasan
- Sigue las pautas de arquitectura acordadas
- Tiene un [tamaño manejable](https://geshan.com.np/blog/2019/12/how-to-get-your-pull-request-pr-merged-quickly/)
- No hay código no utilizado, muerto o duplicado
- No perjudica el sistema en su conjunto
- No introduce [complejidad innecesaria](https://youtu.be/kfffy12uQ7g)

#### Hacer un seguimioneto

Regresa a la PR si has solicitado cambios y propon alternativas para resolverlos. Realiza una sesión de [pair programming](https://martinfowler.com/articles/on-pair-programming.html) si hay demasiados cambios solicitados.

## Wrapping up

Adapta esta herramienta a lo que tu equipo realmente necesita. Aborda los problemas con los que tu equipo está lidiando y escribe algo. Si quieres ser profesional, hazlo. Escribe pautas más extensas o [automatiza aún más](https://www.freecodecamp.org/news/how-to-automate-code-reviews-on-github-41be46250712/). Pero no necesitas ir tan lejos para empezar a mejorar las revisiones de código de tu equipo.

Si deberías o no estar haciendo revisiones de código es un tema para otro post.

Muchas gracias a [undraw.co](https://undraw.co) por las ilustraciones. Puedes suscribirte a mi newsletter **With a grain of salt** y recibir un email con novedades cada cierto tiempo.
