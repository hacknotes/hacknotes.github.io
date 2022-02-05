---
title:  "eCPPTv2 - Review"
categories: 
  - Certificaciones
tags:
  - eCPPT
toc: true
toc_label: "Tabla de contenido"
---

El 26 de enero del 2022, aprobé con éxito mi examen de [eLearnSecurity Certified Professional Penetration Tester](https://elearnsecurity.com/product/ecpptv2-certification/) **`(eCPPTv2)`** y he decidido compartir mi experiencia con el examen y como ha sido mi preparación previa con él.

## eCPPTv2 

Esta certificación es el siguiente nivel de la academia [eLearnSecurity](https://elearnsecurity.com/) que a diferencia de su primera certificación [(eJPT)](https://elearnsecurity.com/product/ejpt-certification/) esta no se basa en una prueba de opción múltiple, sino más bien se espera que realice una prueba de penetración real en una red corporativa.

Para lograr aprobar con éxito esta certificación, [eLearnSecurity](https://elearnsecurity.com/) dice que es necesario, pero **`NO`** suficiente comprometer el último activo que se indica en el examen. Por lo cual repito, **`NO`** solo tendrás que llegar al último objetivo sino también hacer un hallazgo de **`TODAS`** las vulnerabilidades que puedas encontrar y posteriormente realizar un informe de carácter profesional en el cual demuestres **`TODO`** el proceso que llevaste a cabo para poder comprometer por completo la red.

Tu informe también debe contar con mitigaciones a las vulnerabilidades que has encontrado y con una parte que contenga un resumen ejecutivo. Yo utilicé la misma [plantilla](https://github.com/hacknotes/h4ckn0tes/blob/main/Reporte/Reporte%20de%20pentesting%20de%20Wreath.pdf) que cree cuando realice una sala de [TryHackme](https://tryhackme.com/) a la cual solo le realice ligeras modificaciones para el informe del examen.

**NOTA:** el informe debe ser escrito en el idioma inglés.
{: .notice--danger}

Existen dos formas de rendir tu examen:

- Ir directo al examen.
- Tomar el curso de [INE](https://my.ine.com/path/9a29e89e-1327-4fe8-a201-031780263fa9) como preparación previa al examen.

A mi forma de ver las cosas diría que el contenido que ofrece [INE](https://my.ine.com/path/9a29e89e-1327-4fe8-a201-031780263fa9) es muy actual y además económico a diferencia de otras certificaciones.

Del contenido oficial de [INE](https://my.ine.com/path/9a29e89e-1327-4fe8-a201-031780263fa9), los módulos como [PowerShell for Pentesters](https://my.ine.com/CyberSecurity/courses/e36303c3/powershell-for-pentesters), [Wi-Fi Security](https://my.ine.com/CyberSecurity/courses/2e911e47/penetration-testing-wi-fi-security) y [Ruby](https://my.ine.com/CyberSecurity/courses/068f7400/penetration-testing-metasploit-ruby) no son evaluados en el examen, pero si tu objetivo no es solo obtener la certificación **`NO`** deberías pasarlos por alto.

## Mi Preparación

Para poder realizar la certificación [eCPPTv2](https://elearnsecurity.com/product/ecpptv2-certification/) yo pude costearme solo un [plan mensual](https://ine.com/pricing) en [INE](https://ine.com/) el cual me costó **`$49`**.

Si ustedes deciden comprar el mismo plan que yo, deben ser muy conscientes que con ese plan **`NO`** podrán acceder a los laboratorios a practicar toda la teoría que han visto en el curso. Este plan únicamente te da acceso al material y repito **`NO`** a sus laboratorios.

Para la parte práctica yo utilicé una plataforma muy recomendada y en gran parte gratuita llamada [TryHackMe](https://tryhackme.com/)

Existen muchas más plataformas en las cuales practicar tus habilidades de pentesting, pero para la certificación **`yo`** sentí que con [TryHackMe](https://tryhackme.com/) era más que suficiente.

Otra de las cosas que hacía, hago y seguiré haciendo sin duda es ver los canales de YouTube de [Julio Ureña](https://www.youtube.com/c/JulioUre%C3%B1a), [IppSec](https://www.youtube.com/c/ippsec), [Víctor García](https://www.youtube.com/c/takito1812) y [S4vitar](https://www.youtube.com/c/s4vitar), que ciertamente para mi son unos recursos muy valiosos y de gran ayuda.

## Examen

Para el examen tienes que pagar un costo total de **$400**, pero para mi suerte lo pude hacer por menos por un descuento que ofrecía [eLearnSecurity](https://elearnsecurity.com/) en su momento, deberás también crear una cuenta en [eLearnSecurity](https://www.caendra.com/signup), en la cual recibirás indicaciones, credenciales de acceso y material para comenzar tu examen.

El examen cuenta con un periodo de 14 días, de los cuales 7 serán el acceso **`máximo`** que puedes obtener al laboratorio del examen y los 7 días restantes es para que puedas realizar el informe.

En el examen tú podrás resetear el laboratorio 4 veces al día como máximo y también si  deseas podrás dejarlo tranquilamente en "pausa" sin perder nada de tu avance y regresar cuando tú lo desees.

## Mi Experiencia

Yo acabé **`todo`** el proceso de certificación en 7 días.

En el primer y segundo día, yo pude comprometer en su totalidad toda la red corporativa, es decir lo hice dentro de los 7 días que tenemos como plazo el acceso al laboratorio.

Al tercer día decidí descansar y no hacer absolutamente nada con respecto al proceso de certificación.

A partir del cuarto día empece a redactar mi informe, que para mí fue lo más difícil del examen, ya que no tenía mucha experiencia realizando informes de ese tipo.

A mí realizar el informe me tomo 4 días.

## Recomendaciones

1. Toma apuntes de todo lo que te parezca más relevante del curso, te será de ayuda en el examen.
2. No te desesperes, cuentas con tiempo más que suficiente para afrontar el examen.
3. La parte de Post Explotación es muy importante, así seas un usuario con máximos privilegios vuelve a enumerar la máquina.
4. El corazón de esta certificación se centra en el **`Pivoting`** por lo cual sin duda alguna les recomiendo realizar la sala [Wreath](https://www.tryhackme.com/room/wreath), créanme con eso ya tienen más del 50% del examen.
5. Para la parte de Buffer Overflow recomiendo las siguientes salas: [Buffer Overflow Prep](https://tryhackme.com/room/bufferoverflowprep), [Gatekeeper](https://tryhackme.com/room/gatekeeper) y [Brainpan 1](https://tryhackme.com/room/brainpan).
6. Tengan lista una plantilla o por lo menos hagan un informe de las máquinas que vayan haciendo, una buena opción y vuelvo y repito es la sala [Wreath](https://www.tryhackme.com/room/wreath), donde te indican como realizar un informe, además que te brindan varios ejemplos de ellos, el mío lo pueden encontrar [aquí](https://github.com/hacknotes/h4ckn0tes/blob/main/Reporte/Reporte%20de%20pentesting%20de%20Wreath.pdf).
7. Para una parte de las vulnerabilidades web pueden realizar el siguiente [laboratorio](https://hacknotes.github.io/vulnerabilidades/sqli2rce/).
8. Revisar el [path](https://hacknotes.github.io/tags/#ecppt) que yo seguí como preparación.

![eCPPTv2 Certificate](/assets/images/ecppt.png)
