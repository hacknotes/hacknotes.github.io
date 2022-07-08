---
title:  "OSCP 2022 - Review"
categories: 
  - Certificaciones
tags:
  - OSCP
toc: true
toc_label: "Tabla de contenido"
---

Esta certificación fue el objetivo principal desde que comencé en este mundo del **hacking ético**. Prácticamente todo este tiempo me he estado preparando para el momento en el que me toque enfrentarme al OSCP y finalmente el 4 de junio del 2022, aprobé con éxito mi examen de [Offensive Security Certified Professional](https://www.offensive-security.com/pwk-oscp/) **`(OSCP)`** y he decidido compartir mi experiencia con el examen y como ha sido mi preparación previa con él.

## OSCP

Esta certificación es impartida por la academia [Offensive Security](https://www.offensive-security.com/) y se dice que esta es la más o una de las más reconocidas en la industria del **`(Ethical Hacking)`** como ellos mismos lo mencionan en su [sitio oficial](https://www.offensive-security.com/pwk-oscp/).

### Características

Esta certificación se caracteriza por:

- Contar con un tiempo límite de 23 horas con 45 minutos para realizar tu prueba de penetración o prueba técnica.
- Tienes 23 horas con 45 minutos más para poder realizar un informe en el cual debes ser capaz de detallar paso a paso todo el procedimiento que has realizado para comprometer las máquinas que se encontraban en tu examen.
- El examen técnico será totalmente supervisado.

### ¿Como aprobar el OSCP?

Si logras realizar cualquiera de estos escenarios podrás obtener tu certificado con éxito.

- Completar el entorno Active Directory `(40 puntos)` + 3 banderas `(30 puntos)`
- Completar el entorno Active Directory `(40 puntos)` + 2 banderas `(20 puntos)` + puntos extra `(10 puntos)`
- Completar las 3 máquinas independientes `(60 puntos)` + puntos extra `(10 puntos)`

Los escenarios descritos con anterioridad **cada uno** suman un total de `70 puntos`, que son los que se necesitan para aprobar el examen. **Tú puedes conseguir 100 puntos**. 
{: .notice--info}

**NOTA:** se recomienda leer detenidamente los siguientes recursos oficiales: [Estructura del examen](https://www.offensive-security.com/offsec/oscp-exam-structure/) y [Guia de examen](https://help.offensive-security.com/hc/en-us/articles/360040165632-OSCP-Exam-Guide).
{: .notice--danger}

## Curso Oficial
Para acceder al curso y material de Offensive Security se tiene a disposición varias formas como:

- Individual Course
- Learn One
- Learn Unlimited

Para mayor información por favor revisar [aquí](https://www.offensive-security.com/pwk-oscp/).
{: .notice--info}

### Material

Para la fecha que yo me inscribí y para mi suerte aún existía el plan de 30 días que era el más barato.

Yo decidí optar por este plan, ya que yo **antes** de tomar el curso oficial yo ya me había estudiado todo su [temario](https://www.offensive-security.com/documentation/penetration-testing-with-kali.pdf) por cuenta propia con recursos gratuitos para cuando llegue el momento de tener acceso ir directo a los laboratorios.

Es por eso que no puedo hablar mucho del material, ya que yo me lo salte por completo. Esto **NO lo recomiendo en absoluto**, tomen en cuenta que yo lo hice por mis limitaciones económicas y por lo que les comente con anterioridad.

Ahora ustedes contaran con 90 días o más que es tiempo más que suficiente para acabar con su contenido, realizar los ejercicios y practicar en el laboratorio.

### Laboratorio

Offensive Security describe al laboratorio como una simulación de un entorno empresarial real.

Este laboratorio cuenta con 70 máquinas aproximadamente, las cuales están distribuidas entre distintos segmentos de red.

### Mi Experiencia

Como les mencione anteriormente yo contaba con un tiempo límite de 30 días de laboratorio por lo cual decidí aprovechar cada día.

Desde el primer día empece a tratar de comprometer todos los activos que más podía, recuerden que este laboratorio simula un entorno empresarial real por lo cual abra activos que los podrás comprometer solos y otros que dependerán de que hayas comprometido alguno previo. La verdad esto para **mí** resulto algo molesto por lo cual al final de mi segunda semana con el laboratorio decidí dejar de utilizarlo y practicar en [Hack the box](https://www.hackthebox.com/), esto lo hice por una semana hasta que un amigo me dijo que escucho que las máquinas retiradas de los anteriores exámenes se encontraban en otra subred, por lo cual decidí regresar al laboratorio y realizar esas máquinas.

Yo consumí 3 semanas de las 4 que tenía y logré comprometer 35 máquinas que según las estadísticas oficiales de Offensive Security me daban un 54% de probabilidad de pasar el examen. **NO** se dejen llevar por estos datos, ya que solo son [estadísticas](https://www.offensive-security.com/offsec/pwk-labs-success/) y no para todos la cantidad de máquinas realizadas será equivalente a un acierto.

### Recomendaciones

- Ciertas máquinas en alguna medida o alguna característica muy especifica se parecían a las que a mí me toco en mi examen, por lo cual recomiendo hacer todas las máquinas posibles que puedan.
- Para poder acoplarse con mayor facilidad al laboratorio recomiendo seguir esta [lista](https://help.offensive-security.com/hc/en-us/articles/360050473812) de máquinas independientes.
- Entra al [discord](https://discord.com/invite/offsec) oficial de Offensive Security donde encontraras a muchas personas que están pasando por tu mismo proceso o que ya han pasado y te pueden ayudar.
- No tengas miedo de optar por pedir una pista para poder resolver una máquina, recuerda que es mejor hacer la máquina con pistas a no hacerla.
- Con la recomendación anterior no quiero decir que abusen de las pistas. Úsenlas solo cuando estén 100% consientes de que han dado su mayor esfuerzo y se han estancados.

Puede ser que en tu examen te toque una máquina parecida a las del laboratorio y si no la resolviste con pistas en tu entrenamiento ¿Como la resolverás en tu examen?. 
{: .notice--danger}

## Mi Preparación

Como se los mencione al principio yo antes de ingresar al curso de Offensive Security me había estudiado su temario por mi cuenta con recursos gratuitos, pero también es cierto que en mi preparación de cara al examen logré obtener otras certificaciones de pentesting como el eJPT y el eCPPTv2.

Si desean ver mi experiencia con las certificaciones mencionadas con anterioridad presiona [aquí](https://hacknotes.github.io/categories/#certificaciones).
{: .notice--info}

A continuación trataré de dar un pequeño resumen de como los siguientes recursos me ayudaron en mi preparación.

### Proving Grounds Practice

Sin duda para **mi** este fue el mejor recurso para mi preparación. Puede ser que todas o la mayoría de máquinas son excelentes para tu preparación, ya que suelen ser similares a las del examen. 

### Hack The Box

Esta es otra excelente plataforma en la cual no solo podrás prepararte de buena manera para tu examen OSCP sino ir más allá.

NOTA: Para los dos recursos anteriores yo use sin duda y recomiendo usar la [lista](https://docs.google.com/spreadsheets/d/1dwSMIAPIam0PuRBkCiDI88pU3yzrqqHkDtBngUHNCw8/edit#gid=1097814569) de máquinas de Tjnull para Hack the Box y Proving Grounds Practice.
{: .notice--danger}

Para la parte de **Active Directory** este recurso es muy bueno aunque algunas máquinas escapan del nivel del examen OSCP, pero es mejor que sobre a que falte ¿no?.

En estos siguientes enlaces ustedes deberán filtrar por **Active Directory** para obtener un listado de máquinas que podrán realizar para aprender y poner en práctica sus conocimientos.

- [Ippsec](https://ippsec.rocks/?#)
- [S4vitar](https://htbmachines.github.io/)

### TryHackme

Este recurso es muy bueno si estás comenzado en este mundo, pero además también hay unos excelentes laboratorios donde podrás aprender y poner en práctica tus conocimientos de **Active Directory**, incluso según mi punto de vista el contenido de estos laboratorios es más avanzado que lo que verás en el OSCP, así que están muy bien.

Filtra en TryHackme por **Active Directory** y **Mimikatz**, me lo vas a agradecer.
{: .notice--danger}

### Pentester Academy

Si quieres profundizar más en relación a **Active Directory** esta academia te ayudará en eso de una manera asombrosa. Yo tuve la oportunidad de tomar el curso [Attacking and Defending Active Directory](https://www.pentesteracademy.com/course?id=47).

Si dominas los temas vistos ahí estarás más que listo para tu examen con relación a **Active Directory**.

## Examen

Unos días antes de tu examen te deberá llegar un correo recordándote la fecha y hora de tu examen.

Al examen se recomienda estar con unos 15 minutos antes tener tu cédula, DNI, pasaporte o algún documento de identidad listo para presentarlo, además se recomienda también tener una copia escaneada de cualquiera de estos documentos.

Recuerden que el examen  dura 23 horas con 45 minutos en los cuales ustedes deberán sacar un mínimo de 70 puntos. Algo muy importante a tomar en cuenta es que si ustedes no optaron por los 10 puntos extras tienen la obligación de comprometer en su **totalidad** el entorno Active Directory que consta de `3` máquinas.

Más detalles en su [guía oficial](https://help.offensive-security.com/hc/en-us/articles/360040165632-OSCP-Exam-Guide#section-2-exam-information) del examen.
{: .notice--info}

### Supervisión

Esta es la parte que he escuchado que a muchas personas les ocasiona un poco de temor, ya que **las** personas  encargadas estarán vigilando todo lo que haces en tu máquina desde que inicia hasta que finaliza tu examen.

Antes de comenzar con tu examen se te pedirá que muestres tu identificación a la cámara, posterior a eso tendrás que acercar tu cara a la cámara y para finalizar tendrás que mostrar **todo** el lugar donde realizaras el examen, incluso debajo de tu escritorio.

Para mí este proceso no fue nada fuera de lo normal. Las personas que me estuvieron supervisando durante mi examen siempre fueron muy atentas y estaban siempre pendientes a cualquier pregunta.

Durante el examen si deseas salir o descansar lo único que deberás hacer es escribir al supervisor de turno y podrás salir. De igual manera al volver a tu examen antes de seguir con él debes avisar que estás de vuelta a tu supervisor.

Con respecto a la cámara puedes solicitar que se apague en cualquier momento, yo solo lo hice al ir a dormir.

### Informe

Luego de tus 23 horas con 45 minutos de examen práctico tendrás el mismo tiempo para poder realizar tu informe, en este deberás detallar paso a paso todo el procedimiento que has seguido para poder comprometer las máquinas. Es muy importante leer por completo los [requisitos para el reporte](https://help.offensive-security.com/hc/en-us/articles/360046787731-PEN-200-Reporting-Requirements#how-should-i-be-managing-the-documentation-and-reporting-in-pwk). Aquí se te indicará todos los requerimientos que se debe tener en cuenta para poder realizar el informe.

### Mi Experiencia

Yo decidí programar mi examen para después de mi almuerzo, es decir mi examen comenzaría a las `12:00 pm` y terminaría a las `11:45 am`. La idea de esto según mis planes era acabar el entorno de **Active Directory** a las `10:00 pm` para luego dormir hasta las `05:00 am` para tener hasta las `11:45 am` para poder realizar las 3 máquinas **independientes**.

Al principio mi plan funcionaba de maravilla incluso mejor de lo que esperaba, ya que el entorno active directory lo acabe en su totalidad a las `07:00 pm` es decir `3` horas antes de lo que tenía planificado por lo cual decidí aprovechar ese tiempo ganado para empezar con las `3` máquinas independientes.

La verdad estaba muy confiado, ya que dentro de las máquinas independientes había una que se debía abusar de un **Buffer Overflow** y la verdad yo me sentía muy cómodo con eso, ya que los practique mucho, incluso llegue hacer algunas [publicaciones](https://github.com/hacknotes/h4ckn0tes/tree/main/Buffer%20Overflow) de Buffer Overflow por lo cual ya me hice la idea de que me iría a dormir a las `10:00 pm` con `60` puntos.

Resulta que en lo que me creía más fuerte termino siendo un dolor de cabeza, así que aquí un consejo. **No se confíen y manténganse humildes**.

Tanta fue mi desesperación que al ver que no pude con esa máquina decidí saltarme a la siguiente, de la cual pase tiempo revisándola y no pude encontrar nada útil. Decidí volver a cambiar de máquina y de igual manera no pude conseguir nada de esa máquina no sabía que hacer y empece a pensar que no lo lograría, hasta ese momento no había tenido ningún descanso y aún no había cenado por lo cual decidí dejar algunos escaneos automáticos en las máquinas mientras iba a cenar y descansar un poco. Para ese entonces debió haber sido las `09:00 pm`.

Después de volver de la cena empece mi examen nuevamente con la máquina de **Buffer Overflow**, pero de igual manera seguía con el mismo problema así que a las `11:00 pm` decidí ir a dormir para reanudar mi examen a las `05:00 am`.

A las `05:00 am` sonó mi alarma y me puse manos a la obra. Decidí seguir con la misma máquina, ya que yo estaba convencido de que lo lograría, ya que me había preparado mucho en ese tema, pero de igual manera seguía con los mismos problemas. Fue entonces cuando decidí comenzar totalmente de **`0`** esa máquina y fue ahí cuando me di cuenta de mi error, una vez lo arregle fue cuestión de pocos minutos para poder explotar con éxito el Buffer Overflow, de ahí la escalada de privilegios para mi suerte la había practicado antes en mis máquinas de preparación así que fue muy fácil obtener privilegios elevados, una vez terminada esa máquina logré acumular 60 puntos entonces decidí ir a desayunar, era aproximadamente las `07:00 am`.

Después de desayunar volví a tratar de completar otra máquina más. Después de varias horas logré encontrar un vector que posiblemente era mi entrada a esa máquina. Este vector lo encontré realizando un reconocimiento manual, ya que **nmap** no me reportaba nada.

Lo que había encontrado efectivamente me permitió ingresar a este sistema y obtener 10 puntos más dando como resultado los 70 puntos que se necesita para aprobar. Cuando fui capaz de llegar a esa puntuación sentí un alivio tremendo y toda la presión que sentía desapareció, incluso pedí permiso para ir a contarle a mis padres que había obtenido al fin los 70 puntos y aproveche también para darme un descanso. Ahora que lo pienso ese descanso fue exagerado.

Era aproximadamente las `09:00 am` y estaba pensando en si dejar mi examen ahí y comenzar con mi informe o realizar la máquina por completo. Al final decidí seguir con la máquina la cual logré completarla dando como resultado 80 puntos y faltando una hora de que termine mi examen decidí dejar la última máquina y me puse a revisar que no me haya faltado ninguna imagen, ya que he escuchado que si no presentas las imágenes necesarias de tu proceso podrías llegar a perder tu examen.

### Recomendaciones

- Leer la documentación oficial
- No te confíes, mantente humilde
- No tengas miedo en volver a comenzar una máquina desde 0
- Si crees que algo es vulnerable o tienes la corazonada de lo que estás enumerando podría ser útil para ingresar intenta hacerlo de varias formas, ya que una de ellas podría funcionar.
- Algo muy importante. **INTENTA** con distintos exploits. El hecho de que uno no funcione no quiere decir que los otros tampoco lo hagan.
- Es algo obvio, pero no esta demás mencionarlo. **NUNCA TE RINDAS**
- Si no funciona un exploit antes de cambiar a otro o decidir optar por otra vía deberías analizar su código y tratar de entender que es lo que hace.
- Modifica los exploits que encuentres si no funcionan.

Con la última recomendación me refiero a que en mi caso me toco explotar una vulnerabilidad y que el exploit que encontré no me detallaba mucha información, lo único que me decía era que el servicio no era vulnerable, pero mi instinto me decía que era por ahí así que decidí leer lo que hacía el script y me toco modificar y borrar algunas líneas para darme cuenta lo que estaba pasando y gracias a eso encontré lo que me hacía falta para poder abusar de esta vulnerabilidad.
{: .notice--danger}

Si tienen alguna pregunta en particular no duden de contactarme por [LinkedIn](https://www.linkedin.com/in/johnny-chafla) si desean, estaré gustoso de poder ayudarlos.
{: .notice--info}

![OSCP Certificate](/assets/images/oscp/oscp_cert.png)
