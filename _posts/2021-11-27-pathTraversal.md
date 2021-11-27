---
title:  "Path Traversal & Blacklists"
categories: 
  - Vulnerabilidades
tags:
  - Windows
  - Linux
  - OSCP
  - LFI
  - Path Traversal
toc: true
toc_label: "Tabla de contenido"
---

El `Path Traversal` se deriva de la vulnerabilidad [LFI](https://hacknotes.github.io/vulnerabilidades/lfi/) que vimos en la anterior publicación, pero en este caso con ciertas modificaciones que un desarrollador puede implementar como una posible solución a [LFI](https://hacknotes.github.io/vulnerabilidades/lfi/).

## ¿Como encontrarlo?

Como se menciono antes esta vulnerabilidad se deriva de otra, [LFI](https://hacknotes.github.io/vulnerabilidades/lfi/) en este caso. Por lo cual para saber si un servicio web es vulnerable a **`Path Traversal`** se debería contar con ciertos [indicios](https://hacknotes.github.io/vulnerabilidades/lfi/#como-encontrarlo) que indiquen que probablemente exista un [LFI](https://hacknotes.github.io/vulnerabilidades/lfi/) primero. 

Si no obtenemos resultados directamente del [LFI](https://hacknotes.github.io/vulnerabilidades/lfi/), es ahí donde deberemos probar cosas como el `Path Traversal`, entre otras cosas mas que lo veremos en las siguientes publicaciones.
{: .notice--danger}

## Laboratorio

Prácticamente utilizaremos el mismo [laboratorio](https://hacknotes.github.io/vulnerabilidades/lfi/#laboratorio) anterior con unas ligeras modificaciones.

Recomiendo vean como se hizo el anterior [laboratorio](https://hacknotes.github.io/vulnerabilidades/lfi/#laboratorio) ya que ahí explico paso a paso como ponerlo en marcha desde la habilitación de un servicio web.
{: .notice--danger}

Como mencione antes como parte de asegurar un poco mas el código para evitar vulnerabilidades como el [LFI](https://hacknotes.github.io/vulnerabilidades/lfi/), a veces, los desarrolladores especifican rutas absolutas al momento de incluir archivos. 

**Por ejemplo:**

```php
1. include("./languages/" . $_GET['language']);

2. include("lang_" . $_GET['language']);
```

Las declaraciones anteriores tratan de incluir **"solo"** los archivos presentes en la carpeta `languages`.

Dicho esto modificaremos nuestro `index.php` con cualquiera de las sentencias anteriores.

```php
<html>
 <head>
  <title>Path Traversal</title>

   <?php
     echo "<br><pre>";
        $language=$_GET['language'];
        include("./languages/" . $_GET['language']);
     echo "</pre>";
   ?>

 </head>

 <form method="GET" >
     <select name="language">
        <option>english</option>
        <option>spanish</option>
     </select>
   <input type="submit" value="Enviar"/></td>
 </form>
</html>
```

**NOTA:** No olvidar crear la carpeta `languages` y que dentro de ella debemos ubicar nuestros ficheros.
{: .notice--danger}

**Estructura de ficheros**

![Estructura de ficheros](/assets/images/pathTraversal/estructura.png)

## Explotación

Una vez realizado todo lo anterior debemos dirigirnos a `http://localhost/lfi/` en mi caso.

Ahí podremos observar nuestro sitio web y su funcionalidad para incluir archivos, en este `"caso"` para el idioma.

![Textos](/assets/images/lfi/textos.png)

Como sabemos que este servicio web esta incluyendo archivos locales de la maquina, podríamos probar si podemos incluir archivos distintos a los del idioma `(english, spanish)` en este caso.

Un archivo común a enumerar en maquinas linux es el `/etc/passwd` y en maquinas windows el `C:\Windows\boot.ini`.

![/etc/passwd](/assets/images/lfi/nullbytetc_passwd.png)

Como hemos visto, **NO** hemos sido capaces de enumerar otros archivos locales del servidor web que **NO** pertenecen o se encuentran dentro de la carpeta **languages**.

Es ahí cuando entra en juego **Path Traversal**.

Sabiendo que posiblemente este servicio sea vulnerable a LFI gracias a los [indicios](https://hacknotes.github.io/vulnerabilidades/lfi/#como-encontrarlo) que podemos ver en la **url** deberíamos probar algunas cosas como el uso de [Null Bytes](https://hacknotes.github.io/vulnerabilidades/lfi/#null-bytes), Path traversal, entre otras cosas.

Para llevar acabo el uso de Path Traversal es tan sencillo como ir directorios atrás con el uso de **`../`** seguido del fichero que deseamos enumerar.

### Resultado

![Path Traversal](/assets/images/pathTraversal/pathTraversalResult.png)

En este caso **SI** obtenemos el contenido del fichero que deseamos enumerar y que no se encuentra en la carpeta **`languages`**. Esto esto es gracias a Path traversal.

**NOTA:** Es muy importante saber que se pueden combinar vulnerabilidades, por ejemplo tranquilamente y **deberíamos** probar, sí con el `Path traversal` no obtenemos resultados, combinarlo con [Null bytes](https://hacknotes.github.io/vulnerabilidades/lfi/#null-bytes).
{: .notice--danger}

## Blacklists

Un tipo de mitigación para poder controlar el `Path Traversal` es el uso de listas negras `(Blacklists)`, por ejemplo los scripts pueden emplear técnicas de búsqueda y reemplazo para evitar esta vulnerabilidad.

Para este ejemplo tendremos que modificar el fichero `index.php` aumentando el siguiente filtro para remplazar la cadena `../`

```php
$language = str_replace('../', '', $_GET['language']);
```

**index.php**

```php
<html>
 <head>
  <title>Path Traversal</title>

   <?php
        $language=$_GET['language'];
	$language = str_replace('../', '', $_GET['language']);

	echo "$language";
	echo "<br><pre>";
          include("./languages/" . $language);
	echo "</pre>";
   ?>

 </head>

 <form method="GET" >
     <select name="language">
        <option>english</option>
        <option>spanish</option>
     </select>
   <input type="submit" value="Enviar"/></td>
 </form>
</html>
```

**NOTA:** Además de agregar el filtro agregué un simple `echo "$language";` para que sea mas fácil entender que es lo que estamos tramitando por medio de la `url` y que esta  haciendo nuestro filtro.
{: .notice--danger}

![Black list](/assets/images/pathTraversal/blackList.png)

Vemos que evidentemente si le pasamos un fichero que existe este carga correctamente e imprime lo que le estamos pasando como parámetro al servicio web. Por otro lado cuando el fichero no existe no carga nada pero de igual manera imprime lo que le estamos pasando como parámetro al servicio web.

Dicho esto recordemos que para poder listar contenido fuera de la carpeta `(languages)` en los que estos ficheros se encuentran, empleamos [Path traversal](http://hacknotes.github.io/vulnerabilidades/pathTraversal/#resultado) `(../)`.

![Filtro Black list](/assets/images/pathTraversal/filtro.png)

Vemos que en esta ocasión no se nos lista el contenido de nuestro fichero por mas que hemos utilizado Path traversal y eso es gracias a nuestro [filtro](http://hacknotes.github.io/vulnerabilidades/pathTraversal/#blacklists) anterior que elimina la cadena `../` y esto lo podemos corroborar con lo que se imprime por pantalla.

### Explotación

Como sabemos que el filtro elimina la cadena `../` simplemente podríamos agregar el doble de puntos y el doble de barras, para que al momento de eliminarlas aun quede la estructura necesaria `../` para la carga de nuestro fichero.

Esto se debe a que nuestro filtro no esta eliminando la cadena de manera recursiva.

![ Black list Bypass](/assets/images/pathTraversal/result.png)

Existen aun mas formas de burlar estos filtros, algunos ejemplos los podrás ver [aquí](https://github.com/hacknotes/h4ckn0tes/tree/main/Local%20File%20Inclusion/Path%20Traversal).

**NOTA:** Se recomienda usar una función nativa del lenguaje de programación que se este empleando como filtro para realizar esta acción, ya que si crea su propia función, es posible que no esté contemplando todas las variables para evitar un caso de `Path Traversal`.
{: .notice--danger}
