tags: #facil #rce #file-upload 
____
## Reconocimiento

Comenzamos haciendo un escaneo básico de puertos, para conocer cuales están abiertos y buscar un vector de ataque. Para ello usamos el siguiente comando:
```bash
nmap -sS -p- 172.17.0.2 -oN ports -n -Pn --min-rate 5000 --open
```

Esto nos permite exportar al fichero **ports** todos los puertos en formato nmap. Obtenemos lo siguiente:

![Pasted image 20251014050656](../../../../Anexos/Pasted%20image%2020251014050656.png)

Ahora, vamos a lanzar el siguiente comando para averiguar cuál es la versión del servicio que corre por el puerto 80 y también lanzar unos scripts de nmap parra aplicar un reconocimiento:

```bash
nmap -sCV -p80 172.17.0.2 -oN version -n -Pn --min-rate 5000
```

![Pasted image 20251014050716](../../../../Anexos/Pasted%20image%2020251014050716.png)

Ahora vamos a entrar al sitio web para averiguar de qué se trata:

![Pasted image 20251014050741](../../../../Anexos/Pasted%20image%2020251014050741.png)

Vamos a aplicar fuzzing para descubrir directorios y archivos ocultos:

```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x .php,.xml,.txt,.html -t 10 -o dirs.txt
```

No encontramos nada. Miramos el código fuente de la web y vemos lo siguiente:

![Pasted image 20251014052247](../../../../Anexos/Pasted%20image%2020251014052247.png)

Accedemos al directorio:

![Pasted image 20251014052349](../../../../Anexos/Pasted%20image%2020251014052349.png)

Vemos el archivo .php y tratamos de acceder a él:

![Pasted image 20251014052430](../../../../Anexos/Pasted%20image%2020251014052430.png)

Vamos a ver como se tramita la data al servidor:

![Pasted image 20251014053456](../../../../Anexos/Pasted%20image%2020251014053456.png)

Ahora vamos a usar hydra para intentar sacar la contraseña por fuerza bruta:

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt "http-post-form://172.17.0.2/nibbleblog/admin.php:username=^USER^&password=^PASS^:Invalid username or password"
```

![Pasted image 20251014053625](../../../../Anexos/Pasted%20image%2020251014053625.png)

Son falsos positivos y la página nos bloquea. Al final, probando con admin:admin ha funcionado:

![Pasted image 20251014054509](../../../../Anexos/Pasted%20image%2020251014054509.png)

En la sección de plugins, vemos que hay uno llamado MyImage que nos permite subir archivos, así que vamos a probar subir un archivo malicioso php para intentar una RCE.

![Pasted image 20251014073256](../../../../Anexos/Pasted%20image%2020251014073256.png)

![Pasted image 20251014070858](../../../../Anexos/Pasted%20image%2020251014070858.png)

Probamos subir el archivo, con éxito, y vemos que mientras busco la imagen, descubro que tengo directory listing:
Vamos a la siguiente ruta:

![Pasted image 20251014072632](../../../../Anexos/Pasted%20image%2020251014072632.png)
``
Accedo a la imagen y usando el parámetro cmd ejecuto whoami:

![Pasted image 20251014072700](../../../../Anexos/Pasted%20image%2020251014072700.png)

Ahora ejecutaremos una reverse shell para entablar una conexión con nuestra máquina atacante:

![Pasted image 20251014073014](../../../../Anexos/Pasted%20image%2020251014073014.png)

Obtenemos acceso a la shell:

![Pasted image 20251014073328.png](Pasted%20image%2020251014073328.png)

Haciendo un sudo -l vemos lo siguiente:

![Pasted image 20251014062629.png](Pasted%20image%2020251014062629.png)

Podemos convertirnos al usuario chocolate sin necesitad de contraseña usando el binario php, así que ejecutamos lo siguiente:

![Pasted image 20251014074329](../../../../Anexos/Pasted%20image%2020251014074329.png)

![Pasted image 20251014074309.png](Pasted%20image%2020251014074309.png)

Ahora, ejecutando el comando ps -aux, vemos lo siguiente:

![Pasted image 20251014080303.png](Pasted%20image%2020251014080303.png)

Root está ejecutando un script en /opt, así que vamos a verlo:

![Pasted image 20251014080347.png](Pasted%20image%2020251014080347.png)

Vamos a modificar el contenido añadiendo una orden en PHP que le de permisos SUID a la bash:

```bash
echo '<?php exec("chmod u+s /bin/bash");?>' > script.php
```

> NOTA: Poner el echo con comillas simples para que no haga conflicto con las comillas del comando en la secuencia php.

![Pasted image 20251014082033](../../../../Anexos/Pasted%20image%2020251014082033.png)

Ahora solo hay que ejecutar **bash -p** :

![Pasted image 20251014082110](../../../../Anexos/Pasted%20image%2020251014082110.png)




