---
title: Gestión de Incidencias. No todo es crítico
updated: 2020-07-28 08:00
comments: true
mailchimp: true
layout: post
image: /images/alert.png
excerpt: En algún momento te vas a cansar de que cualquier problema sea crítico y urgente. Para resolver eso, empieza a gestionar las incidencias de una forma ordenada.
---

Puedes leer este post en inglés [aquí](/incident-management).

Llega un momento en la vida de un equipo de _Ingeniería de Producto_ en el que te vas a cansar de que cualquier [bug](https://xkcd.com/1700/) o problema sea urgente y crítico. Sobre todo después de todos esos mensajes en Slack a las tantas de la noche. Llegados a este punto, nadie sabe qué problema es una incidencia urgente, qué puede esperar hasta la mañana siguiente y qué ni siquiera vale la pena resolver. Esto perjudica la cultura del equipo y el propio negocio. Cuando todo es urgente, nada lo es y como resultado, a nadie le importa tu mensaje de Slack.

La Gestión de Incidencias no es un proceso que surje de forma natural. De hecho, es un mecanismo de defensa contra el [caos](https://www.youtube.com/watch?v=GdTMuivYF30). El caos sí que es surje de forma natural. Es física.

No puedes arreglarlo todo de golpe, pero necesitas empezar en alguna parte. Este post va de cómo pasar de no tener ningún proceso para gestionar incidencias a tener algo.

Puedes suscribirte a mi newsletter **With a grain of salt** y recibir un email con novedades cada cierto tiempo.

{% include mailchimp.html %}

## Por dónde empezar

Pudes tener el impulso de adoptar procesos famosos como el de [Pager Duty](https://response.pagerduty.com/) o [el de Atlassian](https://www.atlassian.com/incident-management/handbook/incident-response). O incluso copiar este post tal cual. Pero ten cuidado. Estoy escribiendo lo que ha funcionado para mi equipo. No lo copies todo sin más. Tienes un cerebro. Úsalo.

Mi equipo tiene todo esto escrito en una página de Counfluence. Tenerlo escrito no es suficiente. Ni de lejos. Tienes que educar a las personas que gestionan las in cidencias y a las personas que las suelen reportar. Organiza charlas y talleres con ejemplos de la vida real. Asegúrate que todos entiendan el proceso que está a punto de establecer.

Eso de que todo es crítico y urgente es lo primero que necesitas arreglar. Para eso necesitas definir qué es realmente una [incidencia](https://xkcd.com/838/) y algunos niveles de gravedad e impacto para poder saber qué hacer en cada caso. Da ejemplos de la vida real e instrucciones sobre cómo informar cada incidencia. No te preocupes si el proceso no es perfecto. Con cada incidencia el equipo aprenderá y lo mejorará iterativamente.

![](/images/alert.png)

## Qué es una incidencia

Un incidencia es un evento que causa la interrupción de un servicio o una reducción en la calidad de un servicio que requiere una respuesta de emergencia.

Eso últio es importante. _Respuesta de emergencia_. Un incidencia tiene tal impacto, que le da a alguien el derecho de interrumpir la vida de otra persona.

## Niveles de gravedad

Cuando detectes una incidencia, analiza el impacto y aplica un nivel de gravedad. ¿Qué es la gravedad? La gravedad de un incidencia viene determinada por el impacto en el sistema. Cuando informes sobre una incidencia, indica el nivel de gravedad, cuál es el problema que has observado y el impacto que está teniendo.

### Sev1

Incidencia crítica que implica un daño irreversible a la reputación de la empresa o a la propia marca. Debe resolverse al instante, incluso si eso significa dejar el servicio inactivo. Esto jamás debería suceder.

Ejemplos: Vulnerabilidad de seguridad que expone los datos de los clientes. Imágenes que muestran contenido violento, y no deberían, aparecen en la página principal. Las cuentas de los clientes han sido hackeadas.

Respuesta típica: llama al Engineering Manager directamente.

### Sev2

_Incidencia crítica_ que afecta activamente la capacidad de muchos clientes para usar el producto. Es un incidencia importante que afecta la funcionalidad de todo el servicio. Afecta al negocio en su conjunto.

Ejemplos: Mostrar grandes diferencias en los precios. Aplicar descuentos que no deberían existir a todos los productos. Servicio inactivo.

Respuesta típica: envía un mensaje en Slack en el canal _#radar_. Si nadie responde en 10 minutos, llama al Engineering Manager. El equipo responsable debería desplegar una solución lo antes posible.

### Sev3

Incidencia menor que impacta a los clientes de forma parcial. La incidencia se convierte en prioridad número uno del equipo responsable del servicio. No requiere atención inmediata si la incidencia ocurre fuera del horario laboral.

Ejemplos: Una categoría de productos no funciona. No se envían correos electrónicos. Las notificaciones push han dejado de funcionar.

Respuesta típica: Envía un mensaje en Slack en el canal #radar. El equipo responsable lo antenderá en cuanto esté conectado.

### Sev4

Problemas menores, cosméticos o bugs que no afectan la capacidad de los clientes para usar el producto. Son incidencias que no tienen un impacto significativo en el negocio y que no interrumpen el flujo de trabajo del equipo.

Ejemplos: Un cliente no recibió un email de confirmación de compra. Un cliente ha comprado un artículo sin stock. La imagen de un producto es incorrecta.

Respuesta típica: Sólo en horario laboral. Habla con el _Product Owner_ y el _Tech Lead_ responsables del servicio o funcionalidad.

![](/images/bug_fixing.png)

Una incidencia bajar de nivel de gravedad si se alarga demasiado en el tiempo. Si eso llegase a pasar, vas a tener que tomar acciones más drásticas.

## Respuesta ante una incidencia

Mantén la calma. No metas presión a nadie. Las personas trabajan y se comunican peor bajo presión.

De primeras no necesitas todos los roles en un proceso maduro de _Gestión de Incidencias_. Pero el que sí necesitas es el **Communications Manager** o **Communications Lead**. Una persona toma este rol durante una incidencia. Mantiene a todos informados sobre lo que está pasando. Lo que se ha detectado, lo que se está investigando, lo que el equipo ha encontrado, el plazo hasta la resolución, las acciones que se están tomando, etc.

Envía mensajes cada pocos minutos. Parece demasiado, pero confía en mi, no lo es. Si no comunicas regularmente, las personas se ponen nerviosas. Incluso un mensaje tipo _"Todavía estamos investigando."_ ayuda a mantener los nervios bajo control. Cuando estás en modo de crisis, cualquier comunicación es poca.

Más adelante, puedes añadir otros roles como **Comandante de Incidencia** o **Adjunto**, establecer **procesos de guardias**, etc. Todo depende de tu contexto.

## Wrapping up

Empieza con poquito. Algo es mejor que nada. Y hecho es mejor que perfecto. Educa y da tiempo a las personas para comprender y utilizar el proceso. Utiliza cada _Postmortem_ para analizar el proceso de _Gestión de Incidencias_ e intenta mejorarlo de forma iterativa.

Muchas gracias a [undraw.co](https://undraw.co) por las ilustraciones. Puedes suscribirte a mi newsletter **With a grain of salt** y recibir un email con novedades cada cierto tiempo.
