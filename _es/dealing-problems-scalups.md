---
title: Lidiando con los problemas de las scaleups
updated: 2021-02-15 00:00
comments: true
mailchimp: true
layout: post
image: /images/scalups_cover.png
excerpt: Cómo lidiar con algunos de los problemas que aparecen gracias a lo que sacrificas cuando escalas una startup tan agresivamente rápido?
---

Puedes leer este post en inglés [aquí](/dealing-problems-scalups).

[Ried Hoffman](https://twitter.com/reidhoffman) habla en su libro [Blitzscaling](https://www.blitzscaling.com/) sobre los sacrificios que una startup debe hacer para poder escalar de forma agresiva. Y si tu trabajo es liderar un equipo y te unes a una empresa que está escalando de esa manera, nadie te dice qué se sacrificó y qué se está sacrificando. Pero no te preocupes. Lo vas a descubrir de una forma u otra. Tienes que hacerlo.

![](/images/scalups_cover.png)

Pueden haber problemas estructurales, falta de información o documentación, falta de confianza entre equipos, metas de negocio mal definidas, falta de una estrategia unificada de ingeniería o producto, problemas de comunicación entre áreas, grandes dependencias entre equipos, mucha burocracia y un largo etcétera. Me he enfrentado a algunos de estos problemas. Traté de resolver algunos, tuve que aceptar algunos y todavía estoy frustrado con otros. Quiero compartir cómo identificar y lidiar con algunos de ellos.

## La persona recién llegada
Como líder, tu éxito es la [suma del éxito de tu equipo más el éxito de las personas a las que influyes](https://pragprog.com/titles/jsengman/become-an-effective-software-engineering-manager/). Tu principal foco debe ser que tu equipo logre sus objetivos. Tu foco segundario debe ser ganar la credibilidad y la confianza de tus compañeros y managers. Y es verdad que te tendrás que preocupar de ambos al mismo tiempo, pero siempre prioriza a tu equipo. Pide perdón en lugar de pedir permiso. Que tu equipo logre los objetivos de negocio es mejor que complacer a tus compañeros. Si tienes que elegir entre tomar una decisión que beneficie a tu equipo a corto plazo y esperar la aprobación de otros departamentos, elige lo que sea mejor para tu equipo. Esto solo es sostenible a corto plazo. Y hay cierto riesgo porque si tomas una decisión y tu equipo no rinde o falla, otras personas pueden usar tus decisiones como un argumento en tu contra o en contra de tu equipo.

[Comparte este post en 🐦 twitter](https://twitter.com/intent/tweet?text={{page.title}}&url={{site.url}}{{page.url}}&via={{site.twitter_username}}&related={{site.twitter_username}}) o suscríbete a mi newsletter **With a grain of salt** para recibir un email con novedades cada cierto tiempo.

{% include mailchimp.html %}

Algunos de los anti patrones más comunes que verás en los equipos de una scaleup son: falta de claridad en los objetivos de negocio, falta de información o documentación y dependencias mal gestionadas. Vamos a indagar en cada uno de estos tres.

## La estrella polar
Un equipo sin un objetivo de negocio claro está condenado y destinado al fracaso. Tu trabajo como líder es asegurarte de que ese objetivo exista y que tanto el equipo como los stakeholders estén de acuerdo y se sientan cómodos con él.

¿Por qué existiría un equipo sin un objetivo de negocio claro? Quizá porque es difícil coordinar varios objetivos, pero es más fácil coordinar iniciativas. Los objetivos no son binarios. Y es difícil asegurarse que encajan y crean una estrategia comercial unificada y cohesiva. Es por eso que técnicas como los [OKRs no son fáciles](https://productcoalition.com/5-ways-your-company-may-be-misusing-okrs-3d5cdb22aa4e) de implementar.

Cómo identificar rápidamente que este problema existe:

* El equipo no participa en todo el ciclo de creación del producto. Una sola persona está decidiendo qué debe construir el equipo.

* El equipo es evaluado por lo que construyen y no por el valor que aportan. No hay dashboards para medir el éxito o el fracaso.

* Las iteraciones duran semanas o meses. Esa [trampa de construcción constante](https://www.youtube.com/watch?v=jWfI8yXBLsM) crea la ilusión de menor riesgo. Pero qué va.

* Una persona de producto (Product Manager o Product Owner) actúa como un puente entre Ingeniería y Negocio quienes no pueden o no quieren comunicarse.

No vas a solucionar este problema a nivel de toda la empresa, así que empieza pequeño. Pídele a una persona de Negocio que dé una charla al equipo. Asegúrate de que el equipo de Ingeniería comprende y se preocupa por el negocio. Empieza a involucrar a alguien de Negocio en las conversaciones del día a día. Inicia un plan piloto con una sola iniciativa en la que el objetivo de negocio se establezca en conjunto entre Ingeniería y Negocio. Ese es tu [caballo de Troya](https://es.wikipedia.org/wiki/Caballo_de_Troya). Construye a partir de ahí.

## La información es ~~poder~~ tiempo
La documentación es a menudo lo primero que se sacrifica. Normalmente, la organización de la información y el registro del conocimiento no se realizan correctamente o se ignoran por completo. Esto lleva a pasar mucho tiempo buscando información y es peligroso porque crea múltiples hilos de comunicación entre muchas personas.

Cómo identificar rápidamente que este problema existe:

* Las reuniones para adquirir información son bastante frecuentes.

* La información no tiene diferentes niveles. La architectura de sofware se documenta con el [modelo C4](https://c4model.com/) precisamente porque no todo el mundo está interesado en el mismo nivel de detalle.

* La documentación no tiene estándar. Especialmente la documentación que debería servir de interfaz con otros equipos.

* La información se distribuye a través de múltiples servicios y formatos. Docs, Sheets, Confluence o Notion, JIRA, Trello, etc.

* La información se comparte constantemente a través de mensajes anclados en los canales de Slack. Slack se usa a menudo como otra aplicación de chat más y no como un foro. [Este post](https://highgrowthengineering.substack.com/p/taming-slack-) habla de cómo mejorar este problema en concreto.

* Cuando solicitas información, se te redirige a otras personas en lugar de que te pasen directamente un link con la documentación.

* Existen diferentes versiones del mismo documento pero nadie sabe cuál es la versión correcta ni dónde encontrarla.

La mayoría de las veces, la única forma de obtener la información correcta es preguntarle a otra persona. Alguien que lleva más tiempo en la empresa que tú. Así que minimiza la cantidad de personas necesarias para esas interacciones. Hazlo tú si es necesario. O pídele a alguien de tu equipo que te ayude. Organiza en un solo lugar la información relevante para tu equipo. Obtén la mayor cantidad de documentación posible y luego resúmala. Con el tiempo, este trabajo será menos y menos necesario.

![](/images/scalups_documentation.png)

Si tu equipo también tiene el mal comportamiento de no documentar sus decisiones y no registrar información, da un taller. Establece [algunas reglas básicas](/es/dos-and-donts) y evangeliza la cultura del [desarrollo guiado por documentación](https://medium.com/swlh/documentation-driven-development-f9a6d3258e5). Si tu equipo tiene buena documentación, evitarás estar en reuniones inútiles. Cuando alguien te pregunte sobre algo, puedes redirigir a la documentación. Cuanto más documentación tengas, a diferentes niveles, mejor.

## Dependencias
Una scalup es un [sistema socio-técnico](https://es.wikipedia.org/wiki/Sistema_sociot%C3%A9cnico) complejo. Y la mayoría de las veces, los diferentes equipos dependen unos de otros. Una de las razones puede ser que a medida que aumenta la escala, los equipos tienden a especializarse. Para llevar a cabo iniciativas mayores, hay que involucrar a muchos equipos o al menos a muchos sistemas. Otra razón puede ser que un scalup haya pasado por muchas reorganizaciones. Y la estructura actual no refleja la arquitectura de los sistemas. Busca información sobre [la Ley de Conway](https://es.wikipedia.org/wiki/Ley_de_Conway) para entender las consecuencias de esto. A menudo encontrarás confusión acerca de qué equipo es owner de qué servicio. Un equipo o algunas personas pueden tener un conocimiento más profundo de algunos sistemas, pero ya no trabajan activamente en ellos. Tendrás que lidiar también con estas dependencias de conocimiento de dominio.

Cómo identificar rápidamente que este problema existe:

* Dependencias no mapeadas. No existe ningún documento con servicios o equipos que dependan unos de otros.

* El desarrollo de funcionalidades y sus dependencias comienzan al mismo tiempo. Esto no es malo per se. Pero es un desastre si no se hace correctamente.

* Mala o nula comunicación entre equipos dependientes. Y una desconfianza generalizada.

* Las revisiones de código en los sistemas de los que depende tu equipo tardan una eternidad.

* Los repositorios en los que contribuyen muchos equipos no tienen documentación clara.

No intentes eliminar las dependencias. Eso es imposible. Trata de gestionarlas. He encontré dos formas principales de sobrevivir a las dependencias:

### Proveedor de servicios
Intenta resolver las dependencias con antelación. Esto funciona si tu equipo tiene suficiente estructura y visión a largo plazo sobre los objetivos de negocio. La funcionalidad de la que depende tu equipo se retrasará. Sobre todo porque cada equipo prioriza sus propios objetivos. Planifica con eso en mente.

Pero en lugar de pedir una funcionalidad, obtener el _sí_, descubrir que se ha retrasado en el peor momento posible, frustrarte y ser el portador de malas noticias para tu equipo… prueba otra estrategia.

### El pie en la puerta
Pide dos horas a una persona de Ingenieria para que haga pair programming con tu equipo durante dos o tres días. Nadie va a rechazar esa propuesta. Por lo general, las personas están felices de colaborar siempre que el tiempo requerido no sea muy largo. Dos horas al día. Durante una semana. Empezaréis la funcionalidad juntos y, con suerte, tu equipo adquirirá suficiente conocimiento del dominio para que puedan resolver sus propias dependencias. Este enfoque funciona mucho mejor. Garantizado.

![](/images/scalups_dependencies.png)

Antes de siquiera tratar de resolver las dependencias, tendrás que resolver la falta de confianza que a menudo hay entre los miembros de diferentes equipos. En organizaciones tan grandes, es normal que la mayoría de las personas ni se conozcan. Preservar una cultura unificada se vuelve cada vez más difícil. Las personas de otros equipos parecen personas de otras empresas. Verás antipatrones como [Nosotros vs Ellos](https://www.theemotionmachine.com/the-us-vs-them-mentality-how-group-thinking-can-irrationally-divide-us/) e interacciones entre equipos donde falta de confianza y la tensión constante son protagonistas. Esto se vuelve más problemático en un mundo remoto donde las personas interactúan con avatares ignorando a la persona del otro lado de la pantalla. Es fácil deshumanizar y antagonizar a personas que pertenecen a contextos diferentes. Intenta resolver esto para equipos que dependen unos de otros. Organiza algunas sesiones de team building antes de comenzar cualquier tipo de colaboración o interacción. Haz que los miembros de diferentes equipos tengan una café virtual informal. Garantizado que esto funciona mejor que establecer procesos rígidos y reglas arbitrarias. No quiero que se me malinterprete. Los procesos y las reglas sobre los patrones de interacción son importantes. [Team Topologies](https://teamtopologies.com/book) habla largo y tendido sobre esto.


## Credibilidad
A medida que las organizaciones crecen, obtener cualquier aprobación se vuelve más lento. Obtener acceso a algún servicio interno o la aprobación de un presupuesto para usar un producto de terceros puede llevar semanas. Al principio todo te parecerá muy burocrático y es posible que una iniciativa que resuelva un problema estructural nunca vea la luz. Cuando pides permiso, muchas personas de diferentes áreas querrán intervenir y discutir los detalles contigo. Legal, Compliance, Seguridad, Negocio, PPM, etc. Las decisiones tomadas de esta forma pueden llevar meses. No caigas en esa trampa. No va a ninguna parte. Trata de encontrar atajos y establecerlos como estándar cuando tengas suficiente credibilidad. Pero, ¿cómo logras eso?

Cuando llegas por primera vez a una empresa, nadie te conoce. No tienes credibilidad y tendrás que ganártela. Nadie confía en ti. Incluso si dicen que lo hacen, no lo hacen. Eres el recién llegado. Pero hay varias formas de generar confianza y empatía y tendrás que hacerlas todas a la vez:

### Construye una red
Empieza por tus compañeros. Otros líderes de tu organización estarán encantados de hablar contigo. ¿Cuál es su historia? ¿Cómo es su día a día? Pregúntales sobre lo que les molesta. ¿Qué resolverían si pudieran? Preocúpate de forma genuina.

### Ayuda a otras personas
En una empresa que está escalando así de rápido, la gente pedirá ayuda constantemente. No te lances para tratar de resolver los problema directamente. Cuando las personas piden ayuda, quieren resolver los problemas por sí mismas con la orientación de alguien. Identifica y elige las oportunidades en las que puedas tener más impacto. Ayuda a otras personas y equipos a tener éxito en sus propias iniciativas y objetivos. Esa es la clave. Ayuda. Escucha el problema o la frustración de alguien e intenta señalarlo en la dirección correcta. Ofrece algo de tiempo de tu equipo para ayudar a otros equipos. Quizás un par de horas de pair programming por semana. La mayoría de las veces no tendrás el tiempo o la capacidad mental para hacer un seguimiento intensivo. Configura un recordatorio para algunas semanas después y preocúpate de los avances.

### Colaborar en iniciativas cross
Es muy raro que una empresa esté en el extremo en el que nadie esté tratando de solucionar nada. Por eso en cualquier startup encontrarás personas que están liderando o tratando de liderar iniciativas para mejorar y resolver los problemas estructurales que vienen con el blitzscaling. Al principio, únete a ellos. Utiliza esas oportunidades para colaborar con otras personas. Haz amistad con cualquiera que esté intentando cambiar el status quo. Si nadie más lo está haciendo, reúne a esas personas. A veces eso es suficiente. Otras veces, necesitarás agregar algo de estructura a estas iniciativas. Y quizá algo de estandarización.

![](/images/scalups_collaboration.png)

La credibilidad te ayudará a tener apoyo temprano cuando intentes resolver cualquier problema que creas que merece tu atención. Tendrás un acceso más rápido a los recursos. La gente no intentará discutir los detalles de tus decisiones e iniciativas. Incluso si no están de acuerdo contigo, podrás poner en marcha cualquier iniciativa. Tu credibilidad también ayudará a su equipo a tener éxito. Otros equipos aprobarán las PRs de tu equipo antes. Podrás abrir puertas para tu equipo que antes estaban cerradas.

## De equipos a unidades de negocio
¿Qué pasa si no estás liderando un equipo pequeño de 5 a 10 personas, sino toda una unidad de negocio de 30 a 50 personas? Es común ver equipos y líderes optimizando para las máximas locales. Si ves que algo funciona dentro de un equipo, ofreceles el desafío de escalar la solución. Pídeles que den una charla sobre cómo resolvieron ese problema en particular. Es importante que estés presente. Eso les dará credibilidad y autoridad para hacer un cambio. Cada equipo tiene sus propias particularidades, por lo que la estandarización, mientras la empresa sigue creciendo a un ritmo acelerado, lleva tiempo. No te frustres si no sale como esperabas. No todas las iniciativas van a ver la luz del día. Asegúrate que las diferentes personas líderes de tu unidad de negocio tengan suficiente confianza entre ellas para que puedan colaborar de manera efectiva.

## Influencia
El último punto que quiero tocar es la influencia. Dentro y fuera de tu empresa. Una scaleup es el entorno perfecto para experimentar y aprender mientras todavía tienes mentores y referentes. Nadie tiene tiempo para controlar todos y cada uno de tus movimientos. Tienes suficiente libertad y autonomía para probar tantas cosas nuevas como quieras. Y aún puedes pedir ayuda. Comparte lo que aprendes. En el formato que mejor te funcione. También ayuda a tu equipo a compartir lo que aprenden y logran. Dentro y fuera de la empresa. Si nunca escribieron un artículo o si nunca han dado una charla, ofrece tu ayuda para prepararse. Comparte el contenido que crean. Dales visibilidad. Debes de ser su promotor. Haz el pastel más grande para todos.

Comienza con algo pequeño, demuestra que funciona y sólo después aumenta la escala. Trabaja junto con otros líderes. Involucra a otras personas. Una scaleup es un entorno con mucho momento. Y si quieres solucionar algún problema, necesitarás más.

[Comparte este post en 🐦 twitter](https://twitter.com/intent/tweet?text={{page.title}}&url={{site.url}}{{page.url}}&via={{site.twitter_username}}&related={{site.twitter_username}}) o suscríbete a mi newsletter **With a grain of salt** para recibir un email con novedades cada cierto tiempo.
