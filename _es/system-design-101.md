---
title: System Design 101
updated: 2021-01-09 00:00
comments: true
mailchimp: true
layout: post
image: /images/system_design_cover.png
excerpt: Conceptos básicos sobre System Design que cualquier Software Engineer debería entender.
---

Puedes leer este post en inglés [aquí](/system-design-101).

Casi todas las entrevistas que hago giran alrededor de [una pequeña historia](storytelling-tips-technical-interviews). Y en casi todas acabo haciendo una simple pregunta: 

> ¿Por qué es tan importante diseñar nuestros servicios para que no tengan estado? Especialmente si se ejecutan en un servidor.

![](/images/system_design_question.png)

Es increíble cuántas personas contestan mal. Y cuando se equivocan, hago una segunda pregunta:

> ¿Qué es un Balanceador de Carga?

Casi todas las personas contestan que DevOps/SRE no es su campo. ¡Esto está mal! !Muy mal¡ ¡Caca!

Cualquier persona que haga Ingeniería de Software (me da igual si te llamas developer, programador o programadora, jedi o incluso  🦄 unicornio) necesita entender al menos lo básico sobre _System Design_. Sin importar su seniority o si su especialidad es frontend, backend, mobile, herramientas de productividad o no-code. 

1. Más que nada porque es esencial para tener un entendimiento holístico y sistémico sobre cómo diferentes servicios y aplicaciones interactúan entre si.
   
2. Pero también porque la infraestructura afecta la forma de programar. Tiene consecuencias y trade-offs. Obviamente.
   
3. Y también porque no es tan difícil. No necesitas saber cómo configurar un _Load Balancer_ en AWS o Google Cloud pero es importante entender qué es, por qué necesitrías uno y cuándo usarlo.

Este es el primer post de una serie (quizá tres) que cubre lo básico sobre _System Design_. Todo esto te ayudará a encontrar mejores soluciones a los problemas que intentas resolver. No voy a entrar en demasiados detalles porque tendría que escribir libros enteros sobre cada tema. Pero sí que voy a dejar muchos recursos para que puedas leer e investigar por tu cuenta.

[Comparte este post en 🐦 Twitter](https://twitter.com/intent/tweet?text={{page.title}}&url={{site.url}}{{page.url}}&via={{site.twitter_username}}&related={{site.twitter_username}}) o suscríbete a mi newsletter **With a grain of salt** para recibir un email con novedades cada cierto tiempo.

{% include mailchimp.html %}

## Un cliente, un servidor y una base de datos entran en una cafetería

El setup más típico que vas a encontrarte por ahí es una aplicación frontend (quizá usa [React](https://reactjs.org/)) que habla con un servicio backend (digamos que es una aplicación basada en [Spring](https://spring.io/), escrita en [Kotlin](https://kotlinlang.org/), que sirve una [API REST](https://restfulapi.net/) y se ejecuta en un contenedor [Docker](https://www.docker.com/)) que a su vez se conecta a una base de datos (quizá [PostgreSQL](https://www.postgresql.org/)). Las tecnologías dan igual. Lo importante es: cliente, servidor y base de datos.

![](/images/system_design_basic.png)

Peeeeeero esta representación del sistema es, en el mejor de los casos, incompleta. La verdad es que no existe solamente una instancia de la aplicación frontend. Eso sería terrible porque significaría que solamente una persona está usando tu producto. Y quizá esa persona seas tú. Lo que ocurre en realidad es que cada persona ejecuta una instancia diferente en su máquina (desktop o mobile) con diferentes condiciones de conectividad usando navegadores diferentes. Y eso que estamos ignorando por completo cosas como [DNS](https://www.cloudflare.com/learning/dns/what-is-dns/).

![](/images/system_design_dns.png)

Esta forma de ver el sistema es un poco más precisa pero todavía está lejos de la realidad. Al menos en cuanto a frontend se refiere. Así que vamos a hablar de [qué pasa cuando escribes una url en un navegador y presionas Enter](https://medium.com/@maneesha.wijesinghe1/what-happens-when-you-type-an-url-in-the-browser-and-press-enter-bb0aa2449c1a).

## Y presionas Enter

Hoy en día las personas conectan su repositorio de [GitHub](http://github.com/stanete) a algún servicio tipo [Netlify](https://www.netlify.com/) y son felices. Pero dando un paso atrás debes saber que todo el contenido de una página web (HTML, CSS, JS y todos los demás assets estáticos), debe estar almacenado en algún rincón oscuro de Internet. Netlify se encarga de esto y de mucho más pero hace no mucho las personas tenían que construir su aplicación frontend y subir el contenido a un servicio de almacenamiento como [AWS S3](https://aws.amazon.com/s3/).

Pero no basta con almacenar el contenido; hay que servirlo de alguna manera y resulta que las [CDNs](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) son muy buenas para ello.

Así es cómo funciona paso a paso:

1. El navegador solicita al DNS (quizá [AWS Route 53](https://aws.amazon.com/es/route53/)) la dirección IP correspondiente a la url. Esa dirección IP corresponde a la CDN (tal vez [AWS Cloudfront](https://docs.aws.amazon.com/es_es/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)).

2. El navegador solicita el contenido a la CDN. Si la CDN no lo tiene en la caché, lo solicita al almacenamiento (tal vez [AWS S3](https://aws.amazon.com/es/s3/)).

3. El almacenamiento devuelve el contenido a la CDN. A veces incluye una cabecera HTTP llamada Time-to-Live (TTL) que indica [cuánto tiempo se debería guardar el contenido en la caché](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Expiration.html).

4. La CDN guarda el contenido en la caché y lo devuelve al navegador. El contenido permanece en la CDN hasta que expire el TTL.

5. Cuando otro navegador solicita el contenido, la CDN lo devuelve directamente desde la caché. Cuando el TTL expire, vuelta a empezar.

![](/images/system_design_cdn.png)

Hoy en día, si empiezas desde cero, probablemente te puedes saltar todo esto. Pero sí que te vas a encontrar esta configuración en la mayoría de aplicaciones frontend que hay por ahí. Así que [aquí tienes un post explicando cómo utilizar AWS S3 y Cloudfront para hosting de una SPA estática](https://paulstamatiou.com/hosting-on-amazon-s3-with-cloudfront/) y [aquí tienes otro explicando cómo migrarlo todo a Netlify](https://paulstamatiou.com/moving-from-amazon-s3-to-netlify/). De nada.

A menos que tu página web tenga muchísimo tráfico, no te vas a tener que preocupar con problemas de escalabilidad en el frontend durante bastante tiempo. ¿Pero qué pasa con el backend?

## Pon un Load Balancer en tu vida

No puedes controlar dónde se ejecutan las diferentes instancias de la aplicación frontend. Pero con los servicios backend es diferente. No sólo puedes controlar cuántas instancias se ejecutan en cualquier momento, sino que también puedes cambiar la configuración del servidor (como RAM o CPU) en el que se ejecuta cada instancia.

Puedes escalar el sistema verticalmente. Sobre todo cuando tienes poco tráfico. Añade más memoria y/o CPU al servidor y ya estaría. Pero como podrás imaginar, esto tiene un límite natural. Los costes aumentan muy rápido porque tanto la memoria como la capacidad de la CPU son bastante caras. 

Escalar el sistema horizontalmente suele ser más deseable cuando tienes mucho tráfico. No quiere decir otra cosa que levantar tantas instancias como sea necesario. Pero esta solución viene con un par de problemas. No te preocupes, tienen solución. ¿Cómo puedes tener un sólo punto de entrada para que el diseño no se filtre al mundo exterior? ¿Cómo puedes equilibrar la carga del tráfico entre todas las instancias? Tienes razón. Usando un [Load Balancer](https://www.nginx.com/resources/glossary/load-balancing/).

![](/images/system_design_load_balancer.png)

Con este setup, los clientes ya no pueden acceder directamente a los servicios backend. Se conectan a la IP pública del Load Balancer que distribuye uniformemente el tráfico y que se comunica internamente a través de IPs privadas.

Escalar horizontalmente tiene beneficios adicionales. Si una instancia se cae, el tráfico se enrutará a las instancias restantes. Si el tráfico crece rápidamente, puedes levantar automáticamente tantas instancias como sea necesario y después apagarlas para controlar los costes.

Y aquí está la pregunta inicial. De nuevo.

> ¿Por qué es tan importante diseñar nuestros servicios para que no tengan estado? Especialmente si se ejecutan en un servidor.

Suponiendo que el sistema está escalado horizontalmente, no se puede garantizar que un cliente acceda a la misma instancia dos veces seguidas. Es cierto que dependiendo del algoritmo del Load Balancer puedes tener cierta consistencia. Pero, ¿qué pasa si una instancia falla por completo? El estado (los datos) se perdería para siempre. Es por eso que el estado se almacena en una base de datos, lo que en algún momento puede convertirse en un problema porque solo hay una instancia. Y ese será probablemente el próximo cuello de botella.

## [Una de las dos cosas difíciles](https://www.karlton.org/2017/12/naming-things-hard/)

> Solo hay dos cosas difíciles en Ciencias de la Computación: invalidación de caché y nombrar cosas. -- Phil Karlton

Puedes eliminar parte de la carga de la base de datos utilizando una [caché](https://aws.amazon.com/caching/), que no es más que una capa de almacenamiento de datos temporal, como [Redis](https://redis.io/topics/introduction) o [Memcached](https://memcached.org/). En general, la caché almacena el resultado de respuestas pesadas o datos a los que se accede con bastante frecuencia. Es mucho más rápida que la base de datos porque almacena los datos en la memoria. Peeeeero, debes tener cuidado: si se reinicia un servidor de caché, todos los datos se perderán. Obviamente.

¿Cómo funciona? 

1. Después de recibir una request, el servicio backend primero verifica si la caché tiene la respuesta disponible.
   
2. Si no la tiene, hace una consulta a la base de datos, almacena la respuesta en la caché y la envía de vuelta al cliente.

3. La próxima vez que un cliente solicite los mismos datos, se devolverán directamente desde la caché sin consultar la base de datos.

Esto también sirve para servicios externos.

![](/images/system_design_cache.png)

Es mejor utilizar la caché cuando los datos se leen con frecuencia pero se modifican con poca frecuencia. Y no te olvides de implementar una política de expiración o expiration policy. Una vez que los datos expiran, se eliminan de la caché. La política de expiración ayuda a mantener los datos de la caché sincronizados con la base de datos, pero no resuelve problemas de consistencia. Invalidar la caché cuando lo necesites es un reto difícil y hablaré sobre ello en un siguiente post.

## La guerra de los clones

Usar una caché mola, pero en algún momento no será suficiente. La base de datos sigue siendo una única instancia. Un [punto único de fallo](https://es.wikipedia.org/wiki/Single_point_of_failure) y un cuello de botella.

Las operaciones de lectura suelen ser más frecuentes que las de escritura. Cuando ese sea el caso puedes usar [replicación](https://es.wikipedia.org/wiki/Replicaci%C3%B3n_(inform%C3%A1tica)). Es una técnica para manejar múltiples instancias de una base de datos, generalmente con una relación principal/secundaria entre la original y las copias. La base de datos principal generalmente admite solo operaciones de escritura, mientras que las copias solo admiten operaciones de lectura.

![](/images/system_design_db_replication.png)

La replicación tiene múltiples ventajas. Vas a tener mejor rendimiento, confiabilidad y alta disponibilidad. Las operaciones de lectura se distribuyen entre las instancias secundarias, los datos nunca se pierden porque se replican en varias ubicaciones. Incluso si la instancia secundaria se desconecta o apaga, aún puedes acceder a los datos de otras ubicaciones.

Si todas las secundarias se desconectan o apagan, las operaciones de lectura se dirigirán temporalmente a la base de datos principal. Cuando al menos una instancia secundaria esté funcionando correctamente, las operaciones de lectura se redireccionarán de nuevo a ella.

Si la instancia principal se desconecta o apaga, se promoverá una base de datos secundaria y se convertirá en la principal. Una nueva instancia reemplazará la antigua base de datos secundaria. Esto es más difícil de lo que parece porque en la vida real los datos pueden estar sincronizados o no. Y probablemente haya que hacer magia oculta para resolver el problema sin perder datos.

Si todas las instancias, principal y secundarias, se caen, tienes un gran problema.

La replicación es solo una de las muchas técnicas para escalar la base de datos horizontalmente. [Sharding](https://www.digitalocean.com/community/tutorials/understanding-database-sharding) es otra.

## La magia siempre ocurre asíncronamente

Algunas de las tareas que ejecutan los servicios backend, como enviar una notificación o procesar una foto, no necesitan ocurrir de inmediato. Se pueden ejecutar de forma asincrónica. Sin embargo, las instrucciones para saber qué tarea ejecutar, deben almacenarse en algún lugar hasta que un servicio esté listo para ejecutarla. Para ello puedes usar una cola de tareas o [task Queue](https://redislabs.com/ebook/part-2-core-concepts/chapter-6-application-components-in-redis/6-4-task-queues/) también llamada cola de mensajes o [message Queue](https://aws.amazon.com/message-queue/). En realidad [hay diferencias entre task Queue y message Queue](https://stackoverflow.com/questions/10075817/message-queue-vs-task-queue-difference) pero voy a ignorarlas por ahora. Puede que hayas oído hablar de [RabittMQ](https://www.rabbitmq.com/), [AWS SQS](https://aws.amazon.com/es/sqs/) o [Celery](https://docs.celeryproject.org/en/stable/).

![](/images/system_design_queue_workers.png)

La arquitectura básica de una cola es simple. Algunos servicios, llamados producers o publishers, crean mensajes y los publican en la cola. Otros servicios, llamados consumers o suscribers, se conectan a la cola y consumen los mensajes uno por uno para realizar las tareas necesarias. El propio mensaje contiene información sobre qué tarea se va a ejecutar y datos para que la tarea se pueda ejecutar con éxito.

> Solo hay dos cosas difíciles en los sistemas distribuidos: 
> 2. Entregar un mensaje exactamente una vez.
> 
> 1. Garantizar el orden de los mensajes.
> 
> 2. Entregar un mensaje exactamente una vez.

Usar una message Queue viene con sus propios problemas. Es difícil asegurarse de que un mensaje se entregará [exactamente una vez](https://bravenewgeek.com/you-cannot-have-exactly-once-delivery/). Y es igualmente difícil garantizar que todos los mensajes se consumen en el orden deseado.

## Un nuevo comienzo

Ahora sí. Esta es una vista más sistémica y holística sobre cómo interactúan los diferentes servicios que lo componen. En términos de escalabilidad, tu producto tiene que tener bastante éxito para que necesites más que esto. Pero todavía no he tratado de temas como rate limiters, seguridad o autenticación.

![](/images/system_design_complete.png)

Además, todavía hay dos monolitos: uno para el frontend y otro para el backend. Aún necesito hablar sobre las implicaciones de tener múltiples servicios interactuando entre sí cuando decidas romper esos monolitos, backends for frontend, microfrontends o renderización del lado del servidor (SSR). En los próximos dos posts cubriré estos y otros temas como múltiples centros de datos, redes privadas virtuales o sharding.

[Comparte este post en 🐦 Twitter](https://twitter.com/intent/tweet?text={{page.title}}&url={{site.url}}{{page.url}}&via={{site.twitter_username}}&related={{site.twitter_username}}) o suscríbete a mi newsletter **With a grain of salt** para recibir un email con novedades cada cierto tiempo.
