# Vacaciones

tags: #muy-facil #hydra

***

Comenzamos la máquina haciendo un escaneo de los puertos y versiones, así como lanzar unos scripts básicos de reconocimiento. Utilizamos el siguiente comando:

```bash
nmap -sSCV -p- --min-rate 5000 -n -Pn -vvv --open -oN scan 172.17.0.2
```

![Pasted image 20251009151550](<../../../.gitbook/assets/Pasted image 20251009151550.png>)

Cuando vamos a analizar el servicio web, vemos que la web está en blanco, pero si analizamos el código fuente, vemos lo siguiente:

Esto nos puede indicar que Camilo sea el admin, pero amos a hacer fuerza bruta para ambos usuarios: Camilo y Juan:

```bash
hydra -l camilo -P /usr/share/seclists/Passwords/xato-net-10-million-passwords-10000.txt -I ssh://172.17.0.2
```

Como vemos en la imagen de arriba, he abierto dos terminales paralelamente para testear la fuerza bruta tanto con camilo como con luis, dando éxito en este caso luis. Accedemos por ssh a su usuario usando el siguiente comando:

```bash
ssh camilo@172.17.0.2
```

![Pasted image 20251009152652](<../../../.gitbook/assets/Pasted image 20251009152652.png>)

Aquí ya estamos como camilo, y además ejecuto el siguiente comando para obtener una mejor shell:

```bash
script /dev/null -c bash
```

Ahora ejecutamos el comando:

```bash
find / --name "correo*" 2>/dev/null
```

![Pasted image 20251009153929](<../../../.gitbook/assets/Pasted image 20251009153929.png>)

Y ahora, lo leeremos:

Si recordamos, este correo fue enviado por juan,, así que nos conectamos a su usuario:

![Pasted image 20251009154200](<../../../.gitbook/assets/Pasted image 20251009154200.png>)

Ejecutando **sudo -l** vemos lo siguiente:

![Pasted image 20251009154241](<../../../.gitbook/assets/Pasted image 20251009154241.png>)

Con una búsqueda en https://gtfobins.github.io/gtfobins/ obtenemos lo siguiente del binario ruby:

Ejecutamos el comando en la máquina:

Somos root!!
