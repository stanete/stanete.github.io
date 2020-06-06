---
title: Dos and Don'ts para un equipo de Ingeniería de Producto
updated: 2020-06-02 08:00
comments: true
mailchimp: true
layout: post
image: /images/research.png
excerpt: El hermano menor de los Principios de Ingeniería son los Dos and Don'ts. Es sólo una lista de reglas para evolucionar el mindset de tu equipo.
---

Puedes leer este post en inglés [aquí](/dos-and-donts).

Ningún equipo de Ingeniería de Producto es perfecto pero todos tienes la necesidad de mejorar. Esa mejora se realiza bajo algún tipo de guía. Existen muchos posts sobre **Principios de Ingeniería**. Personalmente me gustan y me inspiran los de [Monzo](https://monzo.com/blog/2018/06/29/engineering-principles) o [Medium](https://medium.engineering/engineering-values-7143c0db0bd6). Los **Principios de Ingeneniería** son necesarios pero suelen ser genéricos y estátics.

El hermano menor de los **Principios de Ingeniería** son los **Dos and Don'ts**. Es sólo una lista de reglas específicas. Se enfocan en problemas específicos que tiene un equipo más pequeño en un momento determinado. Los **Dos y Don'ts** tienen que recordarse constantemente hasta que formen parte de la mentalidad del equipo. Pueden y van a cambiar con el tiempo. Escríbelos en alguna parte, crea un bot de Slack que se lo recuerde al equipo automáticamente o crea un poster y cuélgalo donde todos puedan verlo. Puedes incluso plasmarlos en un blog post, como lo estoy haciendo yo aquí. Los siguientes **Dos y don'ts** son específicos para mi equipo actual. Los tuyos podrían y quizá deberían ser diferentes. Esta herramineta puede funcionar o no para tu equipo dependiendo de tu contexto. No existen balas de plata y es triste que tenga que hacer este disclaimer.

### Pregunta "por qué" más de una vez y prepárate para lo mismo

Cuando hagas una presentación para exponer una nueva iniciativa de producto o estrategia técnica, prepárate para ser preguntado ["por qué" hasta cinco veces](https://www.youtube.com/watch?v=FJ0eWm5PxkU). Investiga de antemano tan profundamente como sea necesario para tener respuestas, pero sigue siendo pragmático.

### Pregunta por datos y prepárate que te pregunten por datos

En el mundo de la _Ingeniería de Producto_, respaldar tus argumentos con datos es **obligatorio**. No digas "necesitamos construir esa feature". Junto con los **por qué**, reúne y expon los datos para respaldar tus decisiones y crear argumentos convincentes. Si no hay datos, perderás tu credibilidad y respeto como Ingeniero de Software, Product Manager o Diseñador de Producto. Cualquier miembro de un equipo de _Ingeniería de Producto_ tiene la obligación de [crear una cultura basada en datos](https://aws.amazon.com/blogs/enterprise-strategy/how-to-create-a-data-driven-culture/).

![](/images/research.png)

### Investigua más de una posible solución

Trabajar en un equipo de _Ingeniería de Producto_ requiere tanto [creatividad como proceso](https://uxdesign.cc/what-can-pablo-picasso-teach-us-about-product-strategy-586664e128f1). En general, la solución obvia [no la mejor](https://www.youtube.com/watch?v=M68ndaZSKa8). Investiga múltiples soluciones sin juzgarlas, realiza spikes, recopila datos, crea diagramas, escribe pros y contras, haz una lista de tus sesgos y ten un **decision day**.

### Haz pair programming

[Pair programa](https://www.youtube.com/watch?v=k3cJjZiZ-cw) todo lo que puedas y más. Tiene [tantos beneficios](https://martinfowler.com/articles/on-pair-programming.html) que es difícil de encontrar argumentos convincentes en contra. Usa la técnica que mejor se adapte a tu entorno. Perfecto si haces Ping-Pong-TDD. Driver-Navigator también está bien.

Disclaimer. Hacer pair programming tiene retos, específicamente la fatiga mental. [No es obligatorio hacer pair programming todo el tiempo](https://twitter.com/dhh/status/1016398757674577920). Como con cualquier otra herramienta, úsala de la forma en la que te traiga más beneficios.

### Manten tus PRs pequeñas y fácies de revisar

Mantén tus PRs lo más pequeñas posible y tu código tan fácil de entender como sea posible. Como con cualquier otra cosa, evalúa e intenta [mejorar tus code reviews](/improve-code-reviews) de vez en cuando.

### Mantén staging siempre limpio

Múltiples equipos llevan su código a staging, y como de rápido pueda a producción, múltiples veces al día. En cualquier momento, lo que está en master puede ir a producción. Esto no es un problema, así que no intentes crear entornos adicionales para probar tu código. Hay equipos de _Ingeniería de Producto_ por ahí [que han abandonado completamente los entornos de staging](https://launchdarkly.com/blog/staging-servers-are-dead-long-live-a-staging-server/). Piensa en los problemas con antelación y lleva a master código que simplemente funciona. Si no estás seguro, colócalo debajo de una feature flag.

![](/images/version_control.png)

### Asegúrate de que el código que pones producción, funciona

Idealmente, deberías tener [tests end-to-end automatizados en producción](https://medium.com/@copyconstruct/testing-in-production-the-safe-way-18ca102d0ef1). Hasta que eso no sea una realidad, prueba manualmente que no hayas roto nada. Ten en cuenta que, a medida que aumenta la complejidad, las pruebas manuales en producción se vuelven cada vez más difíciles. Cuanto antes empieces a automatizar, mejor.

### Mide lo que pones en producción

"Hecho" significa que una feature se puede medir desde el punto de vista de negocio, producto e ingeniería. Esta es la única forma de [evaluar que lo que construyes tiene valor](/es/focus-on-value). Para ingeniería, puedes empezar con [las cuatro señales de oro](https://landing.google.com/sre/sre-book/chapters/monitoring-distributed-systems/#xref_monitoring_golden-signals) e ir iterando. Para negocio, puedes empezar con algunos de [estas métricas clave](https://www.ycombinator.com/resources/key-metrics) e iterar. Para producto, necesitas encontrar [las métricas que importan](https://www.intercom.com/blog/finding-the-metrics-that-matter-for-your-product/) para tu producto, pero puedes [empezar](https://amplitude.com/blog/2016/06/14/10-steps-behavioral-analytics) con algunas [analíticas de comportamiento](https://mixpanel.com/blog/2018/08/01/behavioral-analytics-guide/) e iterar conforme vayas aprendiendo.

### No te compliques demasiado. Mantén tu código simple

La simplicidad es la característica más [subestimada](https://blog.pragmaticengineer.com/software-architecture-is-overrated/) del diseño de software. Mantenlo lo más simple posible y no dejes que la [entropía](https://www.youtube.com/watch?v=kfffy12uQ7g) se convierta en un problema. Tu futuro yo te lo agradecerá.

![](/images/online_chat.png)

### No esperes que alguien te responda de inmediato en Slack. Nada es urgente. Nunca

[Pues eso](https://basecamp.com/guides/group-chat-problems). Además, no te disculpes por no responder de inmediato.

### No tengas reuniones para todo, a veces un mensaje de Slack es suficiente

Algunos incluso piensan que [las reuniones son tóxicas](https://twitter.com/dhh/status/1242935396356354048?s=20) y las [reuniones en línea](https://www.youtube.com/watch?v=JMOOG7rWTPg) son peor aún. Yo no soy tan radical. Pero si realmente se necesita una reunión, asegúrate de que esté bien planificada: tiene un propósito, documentación para prepararse, una agenda y un resultado.

### Wrapping up

Se supone que estas reglas van moldeando poco a poco al equipo hacia una mentalidad y una cultura de producto más saludables. Forman parte de la utopía y nunca serán respetados el 100% del tiempo. Nuestro entorno es complejo e impredecible y somos mejores que seguir ciegamente una lista de reglas. Se sabio.

Muchas gracias a [undraw.co](https://undraw.co) por las ilustraciones. Puedes suscribirte a mi newsletter **With a grain of salt** y recibir un email con novedades cada cierto tiempo.
