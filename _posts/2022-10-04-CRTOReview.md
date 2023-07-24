---
title:  "CRTO 2022 - Review"
categories: 
  - Certificaciones
tags:
  - CRTO
toc: true
toc_label: "Tabla de contenido"
---

Certified Red Team Operator o CRTO es una certificación que te permitirá aprender y profundizar en las técnicas y herramientas que se utilizan en el Red Team.

El curso de CRTO te enseña desde el compromiso inicial hasta lograr obtener el control total sobre el dominio, pasando por todas las etapas que conlleva el ciclo de vida de una simulación de adversario.

Puedes obtener más información en su [recurso oficial.](https://training.zeropointsecurity.co.uk/courses/red-team-ops)

## CRTO

La academia [Zero Point Security](https://training.zeropointsecurity.co.uk/) imparte varios [cursos](https://training.zeropointsecurity.co.uk/collections/courses), entre ellos el curso de preparación para el CRTO. Este curso tenía un valor de £399.00 que además incluía acceso al laboratorio y un intento de examen en el periodo que yo comencé mi preparación. Es posible que los precios hayan variado, por lo cual recomiendo ver su [recurso oficial](https://training.zeropointsecurity.co.uk/courses/red-team-ops).

[Zero Point Security](https://training.zeropointsecurity.co.uk/) cuenta con distintos métodos de compra:

- Comprar el material del curso y un intento de examen.
- Comprar el material del curso, un intento de examen y acceso al laboratorio.
- Comprar un intento de examen. 

[Zero Point Security](https://training.zeropointsecurity.co.uk/) te permite pagar por partes, hasta 4 cuotas de £99.75 o £91.25 dependiendo del plan que adquieras.
{: .notice--info}

### Características

Esta certificación se caracteriza por:

- Contar con un tiempo límite de 48 horas para realizar examen.
- El tiempo límite puede ser consumido en un periodo de 4 días.
- El examen no es supervisado.
- Tienes que presentar al menos 6 de 8 banderas para aprobar.
- No se debe presentar un informe.
- Uso de [Cobalt Strike.](https://www.cobaltstrike.com/)

## Curso Oficial

El curso está dividido en varios módulos en los cuales podrás aprender diferentes tácticas y herramientas que se usa en el Red Team.

El curso se inicia con una breve introducción de Red Team, para poco a poco ir profundizando en más conceptos como Password Spraying, Uso de Cobalt Strike, VBA Macros, Overpass the Hash, S4U2Self Abuse, AD CS, Diamond Tickets, LAPS, Windows Defender Bypass, entre muchísimas cosas más. [(Temario completo)](https://training.zeropointsecurity.co.uk/courses/red-team-ops)

Algo muy importante a mencionar es que el curso es impartido por [RastaMouse](https://twitter.com/_rastamouse) y además una vez que adquieres el material del curso tendrás acceso de por vida a él. Cabe recalcar que el material se va actualizando con el tiempo, por lo cual se convierte en un recurso muy valioso.

### Laboratorio

El laboratorio cuenta con varias máquinas a las cuales tendrás que ir ganando acceso por medio de los ataques que se muestran en el curso. Además, también cuenta con 2 máquinas de atacante, una de ellas es un Kali Linux.

A tus máquinas de atacante no podrás subir ni descargar recursos, ya que cuentan con todas las herramientas necesarias para llevar a cabo tu preparación y la explotación del laboratorio.

### Mi Experiencia

Sinceramente, para mí es un curso extraordinario, el cual me enseño de manera detallada y muy fácil de entender conceptos que antes se me complicaban asimilar.

Es un curso que cubre todo lo necesario y más para iniciar en el Red Team. Algo a destacar sobre el curso son las partes en las cuales te enseñan a evitar las fallas comunes de OPSEC para realizar los ataques de una manera más sigilosa.

### Recomendaciones

- Aunque suene demasiado obvio, te recomiendo tomar todos los apuntes que sean necesarios.
- Realicen todos los ataques detallados en el material del curso y si aún les sobra tiempo vuélvanlo a hacer, pero esta vez sin la guía oficial y usen sus apuntes.
- Usen el canal oficial de discord si tienen algún problema o duda sobre algún tema en particular.
- Asegúrense de pausar el laboratorio para que no consuman innecesariamente el tiempo del laboratorio.

## Mi Preparación

Yo comencé con mi preparación el mes de julio de 2022 y acabé el curso en el mes de septiembre de 2022. Para ese entonces aún me sobraba varias horas de laboratorio que aún podía consumir, y de hecho lo hice para ajustar y probar el perfil de amenaza que se proporciona como guía para el examen.

En lo personal realmente yo diría que con el contenido del curso estás más que listo para aprobar el examen, siempre y cuando hayas utilizado el material y el laboratorio a conciencia. Sin embargo, a continuación les dejo recursos que pueden ser de su ayuda no solo para el examen.

- [HackTricks](https://book.hacktricks.xyz/welcome/readme)
- [The Hacker Recipes](https://www.thehacker.recipes/)
- [Red Team Notes](https://www.ired.team/)

## Examen

Para poder comenzar con tu examen tendrás que primero [agendarlo](https://training.zeropointsecurity.co.uk/pages/red-team-ops-exam). Inmediatamente, recibirás un correo con la fecha que elegiste dar tu examen.

Tres días antes y un día antes de tu examen recibirás un correo de recordatorio de cuando iniciara tu examen.

Finamente en el día del examen tendrás que acceder de la misma manera que ingresabas al laboratorio y podrás dar inicio al examen.

En el examen te darán una máquina con credenciales de un usuario válidas a nivel del dominio asumiendo que ya se realizó el compromiso inicial. Desde ahí tendrás que ir moviéndote para poder tener acceso a los diferentes equipos de la red.

Recuerda que necesitas 6 banderas como mínimo para aprobar.
{: .notice--info}

### Mi Experiencia

El examen es muy similar al laboratorio del curso, por lo cual repito que si realizaste el laboratorio a conciencia seguramente aprobaras.

Con respecto al ambiente del examen puedo comentar que es muy estable, no fue necesario en mi caso reiniciar ninguna máquina.

En mi caso pude obtener las primeras 7 banderas de manera casi seguidas, pero fue en la bandera 8 donde me estanque y pase varias horas tratando de poder obtener acceso a la última máquina. Después de revisar mis notas y de realizar **2** veces el mismo procedimiento fue que finalmente logre conseguir las 8 banderas.

### Recomendaciones

- Pausa el examen cuando creas necesario, no consumas tiempo de manera innecesaria.
- Guarda las credenciales o hashes que vas obteniendo, usa la sección de Credentials de Cobalt Strike y además guárdalas de manera local por si acaso.
- No te desesperes, tomate descansos para volver con la mente fresca.
- Antes de rendir el examen, asegúrate haber probado en el laboratorio del curso el perfil de amenaza proporcionado al reservar tu examen.
- No dudes en reiniciar las máquinas si crees que es necesario, incluso las máquinas que dan vida al escenario del examen (Admin Box).

Después de aprobar el examen se te otorgará una [insignia](https://eu.badgr.com/public/assertions/wZziwWP7QtaD55aomZ-AoA) y después de completar todo el material del curso se te otorgará un certificado como el de la siguiente imagen.
{: .notice--danger}

![CRTO Certificate](/assets/images/crto/crto.jpg)
