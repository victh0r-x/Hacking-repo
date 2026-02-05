tags: #metasploit 
__________________________________________________
Metasploit es una herramienta por excelencia para la explotación de vulnerabilidades

Pra empezar, lo más básico es el comando search, para buscar cualquier vulnerabilidad.

### Crear persistencia
____________
Es muy importante esta parte, para evitar el riesgo de perder el ataque en caso de una pérdida de conexión ya sea por mi parte o por parte de la víctima.

Se puede usar persistence pero está obsoleto.

Desactivar firewall de windows de forma remota:

```bash
netsh advfirewall set currentprofile state off
```


### Multihandler
______
Se inicia con el comando:

```bash
use /multi/handler
```

### Fuerza bruta SSH, FTP, SMB y WEB LOGIN 
____
Para hacer fuerza bruta con metasploit, primero vamos a buscar el siguiente payload, y posteriormente lo seleccionamos:

```bash
seach ftp_login
search ssh_login
use / search scanner/smb/smb_login 
```

![Pasted image 20251017023503](../Anexos/Pasted%20image%2020251017023503.png)

Ahora, solo debemos rellenar la sección de USERNAME y la de PASS_FILE:

![Pasted image 20251017030417](../Anexos/Pasted%20image%2020251017030417.png)

Ya solo queda esperar:

![Pasted image 20251017031028](../Anexos/Pasted%20image%2020251017031028.png)
