---
title: System Design 102
updated: 2021-01-22 00:00
comments: true
mailchimp: true
layout: post
image: /images/system_design_holistic_cover.png
excerpt: Más conceptos básicos sobre System Design que cualquier Software Engineer debería entender.
---

Puedes leer este post en inglés [aquí](/system-design-102).

Dicen que las segundas partes nunca son tan buenas como [las primeras](/system-design-101). Excepto el Batman de Nolan. Pero lo voy a intentar.

Las personas y las computadoras viven en una especie simbiosis. Y esto no es una coincidencia. En 1960 J. C. R. Licklider [escribió sobre esta simbiosis](https://groups.csail.mit.edu/medg/people/psz/Licklider.html). Después dedicó el resto de su carrera a trabajar en ello y a financiar proyectos relacionados a través de ARPA. [Licklider](https://en.wikipedia.org/wiki/J._C._R._Licklider) fue originalmente psicólogo y el Internet existe porque animó activamente a las personas de su alrededor a construir una [Red de Computadoras Intergaláctica](https://www.kurzweilai.net/memorandum-for-members-and-affiliates-of-the-intergalactic-computer-network). Era un visionario.

¿Por qué te estoy contando esto? Porque hacer software trata de interactuar con las computadoras y darles instrucciones. Al decirles lo que tienen que hacer, las limitaciones y los problemas surgen principalmente porque somos humanos. [Conway](https://en.wikipedia.org/wiki/Conway%27s_law) lo sabía mejor que nadie.

> Las organizaciones dedicadas al diseño de sistemas [...] están condenadas a producir diseños que son copias de las estructuras de comunicación de dichas organizaciones. —  Melvin E. Conway

¡Que no te engañen! En algún momento de tu vida dividirás un monolito. Y será principalmente porque eres humano y no tanto porque tenga sentido. Al hacerlo, entras en el mundo de los [sistemas distribuidos](https://es.wikipedia.org/wiki/Computaci%C3%B3n_distribuida). Y puede que al principio te de miedo, y con razón. Pero sigue leyendo.

![](/images/system_design_scared.png)

[Comparte este post en 🐦 Twitter](https://twitter.com/intent/tweet?text={{page.title}}&url={{site.url}}{{page.url}}&via={{site.twitter_username}}&related={{site.twitter_username}}) o suscríbete a mi newsletter **With a grain of salt** para recibir un email con novedades cada cierto tiempo.

{% include mailchimp.html %}

## [Meiosis](https://es.wikipedia.org/wiki/Meiosis)

Hay muchas formas de dividir un monolito, pero en cualquiera de ellas eliminarás la complejidad de una base de código y la añadirás al sistema. El código que solías invocar con una función ahora está esparcido en varios servicios. Y esos servicios deben comunicarse entre sí. Ese será tu desafío.

![](/images/system_design_split_backend.png)

Si este diagrama te parece desordenado y extraño es porque lo es. La aplicación frontend interactúa con varios servicios de backend.

## [Baldur's Gate](https://es.wikipedia.org/wiki/Baldur%27s_Gate)

Nunca he jugado a este juego, pero imagino que tiene algo que ver con encontrar el camino hacia algún tipo de puerta única. Perdón. Lo que quiero decir en realidad es que, en lugar de exponer el diseño interno del sistema, sería muy bueno tener una sola interfaz. Daría la ilusión de interactuar con un solo servicio de backend. Esa "puerta" proporcionaría una experiencia cohesiva, uniforme y unificada.

¡Buenas noticias! Ya hay personas que han tenido este problema antes que tú. Así que puedes usar un [proxy inverso](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/) llamado [API Gateway](https://www.redhat.com/en/topics/api/what-does-an-api-gateway-do). Algunos de los más famosos son [Kong Gateway](https://konghq.com/kong/) y [el que ofrece AWS](https://aws.amazon.com/es/api-gateway/).

![](/images/system_design_api_gateway.png)

Pero un API Gateway sirve para mucho más:

1. [Autenticación](https://konghq.com/learning-center/api-gateway/api-gateway-authentication/). En lugar de dejar que cada servicio de backend maneje la autenticación, puedes dejar que el API Gateway se ocupe de ello.

2. Regular el trafico. Rate limiting, [throttling](https://docs.aws.amazon.com/es_en/apigateway/latest/developerguide/api-gateway-request-throttling.html) y restricción de tráfico. Más que nada porque probablemente quieras proteger tus servicios de [ataques DDoS](https://es.wikipedia.org/wiki/Ataque_de_denegaci%C3%B3n_de_servicio).

3. Analytics. Visualizar, inspeccionar y supervisar el tráfico de los servicios backend. Porque seguramente querrás entender cómo se utilizan tus APIs.

4. Transformaciones. Puedes transformar tanto peticiones como respuestas sobre la marcha.

5. [Serverless](https://www.serverless.com/amazon-api-gateway). Podrías invocar [funciones lambda](https://www.serverless.com/framework/docs/providers/aws/guide/intro/) como si fueran una API. No miento.


![](/images/system_design_api_gateway_explained.png)


## [Call me maybe](https://youtu.be/fWNaR-rxAic)

Una vez hayas resuelto los problemas de comunicación entre backend y frontend, es hora de resolver los problemas entre los propios servicios backend.

Claro que puedes utilizar llamadas HTTP. Pero si estás monitorizando al menos las [4 Señales de Oro](https://sre.google/sre-book/monitoring-distributed-systems/), eventualmente vas a observar un aumento en la latencia. Espero que esto no te pille por sorpresa. Hacer una llamada HTTP tarda más tiempo que invocar una función. Entonces no, incluso si tu código está aislado, no puedes ignorar el diseño de todo el sistema.

No me malinterpretes. Este no es un mal diseño y en la mayoría de los casos probablemente funcionará durante algún tiempo. Pero eventualmente necesitarás una solución más decente.

En algún momento, la latencia se convertirá en un problema y alguien con más experiencia que tú en System Design te propondrá utilizar algún tipo de comunicación asincrónica.

En el post anterior ya expliqué qué son las [colas de mensajes](https://aws.amazon.com/es/message-queue/) y más específicamente el patrón [cola de tareas](https://redislabs.com/ebook/part-2-core-concepts/chapter-6-application-components-in-redis/6-4-task-queues/), también conocido como [cola de trabajo](https://www.rabbitmq.com/tutorials/tutorial-two-python.html).

![](/images/system_design_queue_workers.png)

Un _publisher_ distribuye tareas a través de una cola de mensajes entre _workers_ que [compiten para consumirlos](https://www.enterpriseintegrationpatterns.com/patterns/messaging/CompetingConsumers.html). Sólo un servicio y una instancia consume el mensaje y ejecuta la tarea/trabajo.

Pero, ¿y si no quieres que haya competición?. ¿Qué pasa si quieres enviar mensajes de forma asincrónica a varios servicios a la vez? Resulta que [RabbitMQ](https://www.rabbitmq.com/), [Apache Kafka](https://kafka.apache.org/) y [Google Pub/Sub](https://cloud.google.com/pubsub/) permiten que un servicio _publisher_ envíe un mensaje a varios servicios _consumers_. Este patrón se conoce como [publish/subscribe](https://www.rabbitmq.com/tutorials/tutorial-three-python.html).

![](/images/system_design_pub_sub_explained.png)

No necesitas preocuparte demasiado por los detalles, pero es bueno entender cómo funciona el [patrón pub/sub](https://thenewstack.io/publish-subscribe-introduction-to-scalable-messaging/) a grandes rasgos. El _publisher_, también llamado _producer_, nunca envía un mensaje directamente a una cola. De hecho, a menudo el _producer_ ni siquiera sabe si el mensaje se entregará a alguna cola.

En cambio, el _producer_ solo envía mensajes a un _exchange_. Un _exchange_ es algo muy simple. Por un lado recibe mensajes de los _producers_ y por otro lado los envía a las colas que haya. Cada servicio tiene su propia cola pero sólo una instancia de cada servicio consume el mensaje.

Si alguna vez alguien menciona algo llamado [bus de eventos](https://docs.microsoft.com/es-es/dotnet/architecture/microservices/multi-container-microservice-net-applications/integration-event-based-microservice-communications), esto es de lo que están hablando. O esto o algo muy parecido. Juntando todas las piezas puedes tener una visión más completa del sistema. Ahora todo vuelve a tener sentido.

![](/images/system_design_holistic.png)


## [Mitosis](https://es.wikipedia.org/wiki/Mitosis)

De vuelta al frontend. Una forma de dividir un monolito frontend podría ser muy simple. Puedes dividirlo literalmente en dos y almacenar los assets estáticos en diferentes sitios.

![](/images/system_design_split_frontend.png)

Peeeero no todo es un camino de rosas. Administrar el estado y la autenticación de una sola aplicación es sencillo. Pero, ¿qué pasa cuando la divides? Sería una experiencia horrible si alguien navegara de una aplicación a otra y necesitara autenticarse de nuevo. La navegación entre aplicaciones debería ser imperceptible.

Podrías pensar que [almacenar un token JWT](https://stormpath.com/blog/where-to-store-your-jwts-cookies-vs-html5-web-storage) en _local storage_ sería una buena idea. Pero no. Stop. Caca. Mal. No hagas eso. El almacenamiento local está diseñado para ser accesible a través de javascript, por lo que no proporciona ninguna protección contra ataques [XSS](https://en.wikipedia.org/wiki/Cross-site_scripting). Es mucho más seguro utilizar una [cookie de sesión http only](https://www.whitehatsec.com/glossary/content/httponly-session-cookie).

## Divíde y vencerás. O no.

Seguro que ya has oído hablar de los [microfrontends](https://martinfowler.com/articles/micro-frontends.html). Se supone que ayudan a escalar el desarrollo para que muchos equipos trabajen simultáneamente en un producto grande y complejo. Puedes incluso [combinar microfrontends con serverless](https://blog.bitsrc.io/serverless-microfrontends-in-aws-999450ed3795). Pero no voy a entrar en detalles para que nadie me tire piedras. Ni tampoco voy a hablar de [server side rendering](https://sbstjn.com/blog/serverless-create-react-app-server-side-rendering-ssr-lambda/). Lo único que quiero es que entiendas que dividir aplicaciones frontend no afecta solo al código. Siempre hay consecuencias de infraestructura.

![](/images/system_design_micro_frontends.png)

Dividir aplicaciones no reducirá la complejidad. Simplemente la **distribuirá** por todo el sistema.

## Saber y ganar

Hoy en día estamos acostumbrados a tratar con sistemas distribuidos. Pero no siempre ha sido así. Permíteme una breve lección de historia. Las computadoras solían funcionar de forma aislada. Eso fue hasta que nació [ARPANET](https://en.wikipedia.org/wiki/ARPANET) en 1969 después de un parto que duró varios años. El primer sistema distribuido. Mas o menos.

Uno de los primeros cuatro nodos fue la Universidad de California, Los Ángeles. Tenían parte de la infraestructura necesaria porque ya habían intentado conectar los campus de la UCLA algunos años antes, pero como no se llevaban bien, fracasaron. Después de ese primer intento, la UCLA redirigió sus fuerzas a otro proyecto que tenía como objetivo **medir** cómo los bits se movían de un lugar a otro en un sistema informático.

[Len Kleinrock](https://en.wikipedia.org/wiki/Leonard_Kleinrock) de la UCLA, quien estuvo involucrado en el diseño de la ARPANET, literalmente dío un puñetazo en la mesa y exigió **medir** todo lo que ocurría en la red. Su argumento fue que ARPANET era un experimento. Y como tal, había que medirlo todo: cuántos paquetes viajaban por la red, dónde se estaban formando los cuellos de botella, qué velocidad y capacidad se podían lograr en la práctica y qué condiciones de tráfico romperían la red.

Si te das cuenta, todo eso es muy similar a las [4 Señales de Oro](https://sre.google/sre-book/monitoring-distributed-systems/) que se atribuyen a Google hoy en día. Nada mas lejos de la verdad pero nos gusta reinventar la rueda cada pocos años.

Las personas de la UCLA estaban interesadas en sacarle provecho a su proyecto, eso es verdad. ¿Pero a qué más da? Gracias a los cimientos de un proyecto fallido y el impulso del proyecto de **monitorización**, la UCLA se convirtió en el **Centro de Medición de Redes** de la ARPANET. Esto fue en 1969. Justo cuando los Rolling Stones eran considerados la banda de rock más grande del mundo. Si hace 50 años, sí, hace 50 años, las personas se preocupaban por la monitorización, ¿por qué no lo harías tú hoy?

## Mírame

No estoy aquí para decirte qué monitorizar. Ese es un tema para un post futuro. Por ahora solo quiero explicarte cómo suelen funcionar las herramientas de monitorización a nivel muy básico.

Las herramientas más tradicionales, basadas en _push_, envían métricas al servidor de métricas, también conocido como _Aggregator_. Por lo general, tienes que instalar un agente junto a cada servicio que quieres monitorizar. A veces en el mismo contendor; otras veces en un [contenedor sidecar](https://medium.com/bb-tutorials-and-thoughts/kubernetes-learn-sidecar-container-pattern-6d8c21f873d). Tienes que configurar cuándo, dónde y con qué frecuencia enviar las métricas. Pero enviar métricas no es el objetivo principal de tus servicios, por lo que en lugar de enviarlas una por una, también tienes que decidir si las métricas deben agregarse y cómo se deben agregar antes de enviarlas. Aunque, si usas algo como [New Relic](https://newrelic.com/resource/key-capabilities-new-relic-apm), no tienes que preocuparse demasiado por los detalles de implementación.

![](/images/system_design_monitoring_push.png)

Pero puedes diseñar el sistema de monitorización de una manera completamente transparente para tus servicios. Las herramientas basadas en _pull_ en vez de _push_ como [Prometheus](https://danielfm.me/prometheus-for-developers/) lo hacen posible. Debido a que la configuración no está vinculada a los servicios, ya no tendrás que re-desplegar tus servicios si quieres cambiarla.

![](/images/system_design_monitoring_pull.png)

Así es como podría funcionar una herramienta de monitorización basada en _pull_:

1. Todos tus servicios deben tener un endpoint `/metrics`. Esto es super común y puedes encontrar muchas librerías y/o frameworks que te lo dan gratis.

2. El _Aggregator_ necesita saber dónde están los servicios a los que pedir las métricas. Puedes definirlos de forma estática pero ¿qué pasaría si una IP cambia? ¿O si un servicio se cae? Quizá sería mejor utilizar algún tipo de [Service Discovery](https://www.nginx.com/blog/service-discovery-in-a-microservices-architecture/) que le diga automáticamente al _Aggregator_ de dónde extraer las métricas. Quizá puedas usar [Consul](https://www.consul.io/use-cases/service-discovery-and-health-checking) para eso.

3. El _Aggregator_ envía peticiones HTTP al endpoint `/metrics` de forma regular. Prometheus llama a eso un _scrape_. La respuesta se analiza y se guarda en el almacenamiento local.

4. Algunos servicios, por la razón que sea, son más difíciles de instrumentar. En esos casos podrías desplegar un _[Exporter](https://prometheus.io/docs/instrumenting/exporters/)_ al lado de ese servicio que proporcionará el endpoint `/metrics`.

5. Imagino que querrás visualizar y analizar las métricas de alguna manera. Hay muchas herramientas que te permiten hacer eso pero [Grafana](https://grafana.com/) es la más famosa. Te permite consultar, visualizar y comprender tus métricas.

6. Algunos agregadores de métricas como Prometheus también se pueden configurar para enviar alertas cuando ocurre algo inesperado. Un _[Alert Manager](https://prometheus.io/docs/alerting/latest/alertmanager/)_ puede recibir alertas del servidor de métricas y convertirlas en notificaciones.

## Léeme

El [logging](https://medium.com/redbox-techblog/building-an-open-data-platform-logging-with-fluentd-and-elasticsearch-4582de868398) es otro mecanismo útil que te da visibilidad sobre el comportamiento de un servicio. Tradicionalmente, era común escribir los registros en un archivo llamado _logfile_. Pero hoy en día un servicio [no debería preocuparse con guardar logs en disco](https://12factor.net/logs). Debería escupir los logs a la salida estándar o _stdout_. Hay muchos beneficios de hacer eso. Durante el desarrollo local, puedes ver los logs en tu terminal.

![](/images/system_design_logging.png)

Mientras que en un entorno de producción, los logs se enviarán a un procesador de logs como [Fluend](https://medium.com/redbox-techblog/building-an-open-data-platform-logging-with-fluentd-and-elasticsearch-4582de868398). Allí se filtrarán y luego se almacenarán en una base de datos como [Elasticsearch](https://www.elastic.co/what-is/elasticsearch). Al igual que con las métricas, querrás buscar, visualizar y analizar los registros con una herramienta como [Kibana](https://www.elastic.co/what-is/kibana).

## ¿Hemos llegado ya?

Sé que hemos hablado de muchos conceptos y herramientas. Y esta última parte sobre las métricas y el logging no es fácil de comprender. No te preocupes. Lee el post de nuevo e indaga en los recursos que te he dejado entre las líneas. 

No hemos terminado. No exactamente. Los sistemas distribuidos manejan datos distribuidos. Y ese será el tema del próximo post de esta serie. Y eso que todavía no hemos hablado de Kubernetes.

[Comparte este post en 🐦 Twitter](https://twitter.com/intent/tweet?text={{page.title}}&url={{site.url}}{{page.url}}&via={{site.twitter_username}}&related={{site.twitter_username}}) o suscríbete a mi newsletter **With a grain of salt** para recibir un email con novedades cada cierto tiempo.
