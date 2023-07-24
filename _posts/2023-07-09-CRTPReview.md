---
title:  "CRTP 2023 - Review"
categories: 
  - Certificaciones
tags:
  - CRTP
toc: true
toc_label: "Tabla de contenido"
---

Hace un tiempo atrás siempre me hacía la pregunta si sería capaz de dejar de depender de distribuciones en Linux para realizar pentesting. Fue entonces cuando decidí tratar de utilizar lo menos posible estas distribuciones o realizar mis trabajos desde un sistema operativo Windows. 

Siendo sincero al principio no sabía por donde comenzar, incluso no podía replicar lo que ya hacía antes, pero esta vez desde Windows. No fue hasta que di con la certificación [CRTP](https://www.alteredsecurity.com/adlab) que todo cambio.

## CRTP

[Certified Red Team Professional](https://www.alteredsecurity.com/adlab) o CRTP te permitirá aprender y profundizar en la enumeración de Active Directory, el mapeo de confianza, la escalada de privilegios de dominio, la persistencia de dominio, los ataques basados ​​en Kerberos (Diamond Ticket, Golden Ticket, Silver Ticket), problemas de ACL, confianzas de servidor SQL, defensas y omisiones de defensas.

El CRTP es impartida por la academia [AlteredSecurity](https://www.alteredsecurity.com/). En el periodo de tiempo en el cual escribo esta publicación puedes optar por diversos métodos de pago, los cuales son:

- $249 por 30 días de laboratorio.
- $379 por 60 días de laboratorio.
- $499 por 90 días de laboratorio.

Te recomiendo visualizar el [recurso oficial](https://www.alteredsecurity.com/adlab) para comprobar sus precios actuales.
{: .notice--danger}

### Características

Esta certificación se caracteriza por:

- Poder presentar el examen en cualquier momento desde la compra del curso.
- Contar con un tiempo límite de 24 horas para realizar examen.
- Tener que realizar un informe detallado y enviarlo, para lo cual cuentas con 48 horas.
- El examen no es supervisado.
- La certificación tiene una validez de 3 años (El examen de renovación es [GRATIS](https://www.alteredsecurity.com/post/renewal-process-for-altered-security-certifications)).
- El contenido principal está enfocado en realizar lo visto en el curso desde una consola de PowerShell.

También existe un manual con el camino para resolver el laboratorio desde [Covenant.](https://github.com/cobbr/Covenant)
{: .notice--info}

## Curso Oficial

El curso está dividido en 10 módulos que van desde la enumeración de Active Directory hasta Defensas y Bypass. Cuenta con 23 objetivos, 59 tareas y 120 horas de tortura, como lo mencionan en su [recurso oficial.](https://www.alteredsecurity.com/adlab)

Algo muy importante a mencionar es que el curso es impartido por [Nikhil Mittal](https://github.com/samratashok), quien es creador de varias herramientas populares utilizadas en el curso de CRTP, así como también en el día a día.

El material del curso se imparte por medio de diapositivas y recursos de audio y video totalmente descargables luego de la compra del curso.

### Laboratorio

El laboratorio cuenta con varias máquinas, así como también con distintos dominios y bosques a los cuales tendrás que ir ganando acceso por medio de los ataques que se muestran en el curso. 

En tu máquina inicial podrás subir y descargar recursos según sea tu necesidad. Las herramientas que se utilizan a lo largo del curso se pueden descargar una vez adquieras el mismo.

### Mi Experiencia

Con respecto al material del curso (diapositivas) me hubiese gustado que estén en un formato en el cual se pueda traducir o al menos se pueda copiar y pegar lo descrito en ellas. Esto en mi caso, ya que yo no domino el idioma inglés. Sin embargo, diría también que las diapositivas se complementan muy bien con los videos del curso, siendo así más fácil de digerir el contenido.

Por otro lado, al hablar del laboratorio yo nunca tuve ningún inconveniente con él, es decir, la experiencia con él fue totalmente estable y agradable. 

Algo que sin duda debo recalcar sobre el laboratorio es que es un entorno de **Windows realista** y además **completamente parcheado**, contando con máquinas Windows Server 2022 y SQL Server 2017. Lo cual te exige a no depender de exploits sino más bien en poder abusar de malas configuraciones, caracteristicas y funcionalidades para comprometer en su totalidad un entorno de Active Directory.

### Recomendaciones

- Es un contenido extenso y los comandos que se presentan en el curso no son fáciles de aprender, por lo cual tomen **todos** los apuntes que sean necesarios.
- Realicen todos los ataques detallados en el material del curso y si aún les sobra tiempo vuélvanlo a hacer, pero esta vez sin la guía oficial y usen sus apuntes.
- No se abrumen con la cantidad de herramientas que se presenta en el curso, elijan la herramienta con la cual se sientan más cómodos.
- Aunque suene que me contradiga antes de elegir una herramienta a su gusto, si les recomiendo que realicen sus ataques con diferentes herramientas con el fin de comparar sus resultados y además siempre es mejor tener un as bajo la manga en caso de que la herramienta que elegiste no funcione como lo esperabas.

## Mi Preparación

Como parte de mi preparación yo elegí el plan de 30 días y decidí ponerme a prueba al realizar el laboratorio completamente por mi cuenta. 

Como resultado logré realizar y obtener el control de cada una de las máquinas que se encontraban en el alcance del laboratorio; sin embargo, también lo volví a hacer, pero esta vez con los recursos y guías que te presentan en el curso. No me avergüenza decir que pese a que logre comprometer todo el laboratorio, se me pasaron algunas vías alternativas de explotar el laboratorio, por lo cual **recomiendo** sin duda que aprovechen todo el material del curso en su totalidad, porque a mi manera de pensar no me sirve de mucho realizar algo de una manera en la cual ya tengo conocimiento y privándome de aprender nuevas herramientas y técnicas.

Es mejor contar con vías alternativas de realizar lo mismo, en el caso de que lo que hacías ya no funcione.
{: .notice--info}

Tengan en cuenta que la metodología que lleve a cabo sobre el laboratorio fue porque ya contaba con experiencia. Como lo mencione antes, no lo recomiendo hacer porque podrían saltarse partes fundamentales del contenido del curso.
{: .notice--danger}

A continuación les dejo recursos que pueden ser de su ayuda no solo para el examen.

- [HackTricks](https://book.hacktricks.xyz/welcome/readme)
- [The Hacker Recipes](https://www.thehacker.recipes/)
- [Red Team Notes](https://www.ired.team/)

## Examen

Para poder comenzar con tu examen lo único que deberías hacer es acceder de la misma manera que se accede al laboratorio y en la parte de tomar tu certificación presionar el botón para empezar tu examen. Una vez es presionado el botón tomará un tiempo en configurarse tu examen, toma en cuenta que una vez hayas decidido comenzar el examen no lo podrás detener.

Una vez tu entorno de examen se acabe de configurar recibirás un correo de notificación de la hora en la cual comienza tu intento de examen y en la página del laboratorio se te generarán credenciales para comenzar. Puedes optar por realizar tu examen desde una VPN o por el mismo navegador.

En el examen te darán una máquina con credenciales de un usuario válidas a nivel del dominio, asumiendo que ya se realizó el compromiso inicial. Desde ahí tendrás que ir moviéndote para poder tener acceso a los diferentes equipos de la red.

Recuerda que en el laboratorio de tu examen no encontraras ninguna herramienta como lo hacías en el laboratorio del curso. Eres tú quien decide que herramientas utilizar y subir al laboratorio.
{: .notice--danger}

### Mi Experiencia

Con respecto al ambiente del examen puedo comentar que es muy estable, no fue necesario en mi caso reiniciar ninguna máquina.

Yo comencé mi examen el 30 de junio aproximadamente a las 8:30 am y logré finalizarlo el mismo día a las 7:50 pm.

Puedo comentarles que el examen es muy similar al laboratorio del curso; sin embargo, **NO** te confíes. En mi caso llegué a un punto en el cual tenía claro el panorama de como completar en su totalidad mi examen, de hecho lo hubiera logrado en muy poco tiempo si no me hubiera estancado en el cómo conseguir una pieza fundamental para seguir el camino que me había trazado.

Luego de haber completado el examen, me sobraban muchas horas más en las cuales podía acceder al examen; sin embargo, decidí no terminar de inmediato con ese tiempo y guardarlo en el caso de necesitar alguna imagen o información para mi reporte final. Yo diría que en el reporte me demore al rededor de unas 8 o 10 horas.

Finalmente, el 2 de julio envié mi reporte y al mismo tiempo se me notifico que los resultados los obtendría en un periodo de 7 días laborables. El 9 de junio recibí un correo de que había aprobado mi examen y si deseaba cambiar el nombre de como se forjaría el certificado, así como también me comunicaron que el certificado lo obtendría en aproximadamente una semana; sin embargo, el mismo día más tarde me llego mi certificado.

Quiero mencionarles que la experiencia que tuve con el soporte y todo en general con la academia [AlteredSecurity](https://www.alteredsecurity.com/) fue increíble, son muy amables, ágiles y rápidos al momento de responderte alguna duda o inquietud. Realmente esto es muy gratificante. Muchas gracias [AlteredSecurity](https://www.alteredsecurity.com/) y [Nikhil Mittal](https://github.com/samratashok).

### Recomendaciones

- Trabaja de manera organizada, toma apuntes de lo que has hecho y lo que piensas hacer para que no se te pase nada por alto. 
- Dibuja el entorno a medida que lo vayas enumerando, yo lo hice con [xmind](https://xmind.app/).
- Guarda las credenciales o hashes que vas obteniendo y guárdalas de manera local por si acaso.
- No te desesperes, tomate descansos para volver con la mente fresca.
- Antes de rendir el examen, asegúrate contar con herramientas las cuales pasen desapercibidas por Windows Defender, al menos en el principio.
- No dudes en reiniciar las máquinas si crees que es necesario.
- Te recomiendo que eches un vistazo a lo siguiente: [amsibypass](https://github.com/surya-dev-singh/AmsiBypass-OpenSession/releases/tag/amsibypass), [uso de mimikatz](https://github.com/ParrotSec/mimikatz#vault--lsadump).

Después de aprobar el examen se te enviará tu certificado.

![CRTP Certificate](/assets/images/crtp/crtp.png)
