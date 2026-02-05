tags: #telnet
_____
## Reconocimiento

No tengo conexión por ping con la máquina, así que haré directamente el escaneo de la IP con nmap;

```bash
nmap -sS -n -Pn -vvv -oN ports -p- --open --min-rate 5000 10.129.27.43
```

Esto nos permite exportar al fichero **ports** todos los puertos en formato nmap. Obtenemos lo siguiente:

![Pasted image 20251213130522](../../../Anexos/Pasted%20image%2020251213130522.png)

Tenemos abierto el puerto 23, que corresponde al servicio telnet. En la descripción de la máquina nos dice que podemos acceder como root con una contraseña en blanco, así que pruebo, y así es:

```bash
telnet 10.129.27.43
```

![Pasted image 20251213131143](../../../Anexos/Pasted%20image%2020251213131143.png)

![Pasted image 20251213131210](../../../Anexos/Pasted%20image%2020251213131210.png)

Root flag conseguida.