 tags: 
____
## Reconocimiento

Comenzamos haciendo un escaneo básico de puertos, para conocer cuales están abiertos y buscar un vector de ataque. Para ello usamos el siguiente comando:
```bash
nmap -sS -p- 172.17.0.2 -oN ports -n -Pn --min-rate 5000 --open
```

Esto nos permite exportar al fichero **ports** todos los puertos en formato nmap. Obtenemos lo siguiente:

![Pasted image 20251014084413](Pasted%20image%2020251014084413.png)

Ahora, vamos a lanzar el siguiente comando para averiguar cuál es la versión del servicio que corre por el puerto 80 y también lanzar unos scripts de nmap parra aplicar un reconocimiento:

```bash
nmap -sCV -p80,22 172.17.0.2 -oN version -n -Pn --min-rate 5000
```

![Pasted image 20251014084432](Pasted%20image%2020251014084432.png)

Ahora vamos a entrar al sitio web para averiguar de qué se trata:

![Pasted image 20251014085123](Pasted%20image%2020251014085123.png)

Pruebo estas credenciales:

admin' or '1'='1 : admin' or '1'='1

![Pasted image 20251014092252](Pasted%20image%2020251014092252.png)

Ahora accedemos al recurso .logs con la siguiente URL:

![Pasted image 20251014093804](Pasted%20image%2020251014093804.png)

Ahora ejecutando ps -aux  vemos también lo siguiente:

![Pasted image 20251014094737](Pasted%20image%2020251014094737.png)

Ejecutamos el comando:

```bash
socat - UNIX-CONNECT:/home/conx/.cache/.sock
```

![Pasted image 20251014095026](Pasted%20image%2020251014095026.png)

Ahora inspeccionamos  que el usuario root está ejecutando una tarea cron.d:

![Pasted image 20251014101146](Pasted%20image%2020251014101146.png)

Vamos a cambiar el contenido de ese script backup.sh por un código malicioso que le otorgue permisos SUID a la bash:

![Pasted image 20251014101619](Pasted%20image%2020251014101619.png)

Una vez hecho y tras esperar unos segundos, se activa el script:

![Pasted image 20251014101914](Pasted%20image%2020251014101914.png)

Ahora ejecutamos /bin/bash -p

![Pasted image 20251014102903](Pasted%20image%2020251014102903.png)
Y somos root!!