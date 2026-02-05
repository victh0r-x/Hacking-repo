tags: 
____
Comenzamos haciendo un escaneo básico de puertos, para conocer cuales están abiertos y buscar un vector de ataque. Para ello usamos el siguiente comando:
```bash
nmap -sCV 172.20.10.4 --min-rate 5000 -vvv -oN version -n -Pn -p21,22,23,25,53,80,111,139,445,512,513,514,1099,1524,2049,2121,3306,3632,5432,5900,6000,6667,8009,8180,8787,33231,35721,54217,55833
```

Esto nos permite exportar al fichero **ports** todos los puertos en formato nmap. Obtenemos lo siguiente:

|Puerto|Estado|Servicio|Flags|TTL|
|--:|:-:|:--|:--|:-:|
|22/tcp|open|ssh|syn-ack|64|
|23/tcp|open|telnet|syn-ack|64|
|25/tcp|open|smtp|syn-ack|64|
|53/tcp|open|domain|syn-ack|64|
|80/tcp|open|http|syn-ack|64|
|111/tcp|open|rpcbind|syn-ack|64|
|139/tcp|open|netbios-ssn|syn-ack|64|
|445/tcp|open|microsoft-ds|syn-ack|64|
|512/tcp|open|exec|syn-ack|64|
|513/tcp|open|login|syn-ack|64|
|514/tcp|open|shell|syn-ack|64|
|1099/tcp|open|rmiregistry|syn-ack|64|
|1524/tcp|open|ingreslock|syn-ack|64|
|2049/tcp|open|nfs|syn-ack|64|
|2121/tcp|open|ccproxy-ftp|syn-ack|64|
|3306/tcp|open|mysql|syn-ack|64|
|3632/tcp|open|distccd|syn-ack|64|
|5432/tcp|open|postgresql|syn-ack|64|
|5900/tcp|open|vnc|syn-ack|64|
|6000/tcp|open|X11|syn-ack|64|
|6667/tcp|open|irc|syn-ack|64|
|6697/tcp|open|ircs-u|syn-ack|64|
|8009/tcp|open|ajp13|syn-ack|64|
|8180/tcp|open|unknown|syn-ack|64|
|8787/tcp|open|msgsrvr|syn-ack|64|
|33231/tcp|open|unknown|syn-ack|64|
|35721/tcp|open|unknown|syn-ack|64|
|54217/tcp|open|unknown|syn-ack|64|
|55833/tcp|open|unknown|syn-ack|64|
## PUERTOS Y SERVICIOS
__________________
### PUERTO 21
____
Ejecutamos el siguiente comando para buscar vulnerabilidades en este puerto:

```bash
nmap --script vuln -p21 172.20.10.4 -oN vuln-21 -n -Pn --min-rate 5000  
```

![Pasted image 20251016125056](../../Anexos/Pasted%20image%2020251016125056.png)

Vemos el servicio vulnerable a una backdoor, así que abrimos metasploit para buscarlo:

![Pasted image 20251016125204](../../Anexos/Pasted%20image%2020251016125204.png)

Seteamos el RHOST y lanzamos el exploit:

![Pasted image 20251016130720](../../Anexos/Pasted%20image%2020251016130720.png)

### PUERTO 25
____
