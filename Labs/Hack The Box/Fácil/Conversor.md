# Conversor

tags:

***

### Reconocimiento

```bash
ping -c 1 10.10.11.92
```

Estamos ante un sistema linux, así que seguimos haciendo un escaneo básico de puertos, para conocer cuales están abiertos y buscar un vector de ataque. Para ello usamos el siguiente comando:

```bash
nmap -sS -p- -vvv -n -Pn --open -oN ports 10.10.11.92 --min-rate 5000
```

Esto nos permite exportar al fichero **ports** todos los puertos en formato nmap. Obtenemos lo siguiente:

![Pasted image 20251102133752](<../../../.gitbook/assets/Pasted image 20251102133752.png>)

Ahora, vamos a lanzar el siguiente comando para averiguar cuál es la versión del servicio que corre por el puerto 80 y también lanzar unos scripts de nmap parra aplicar un reconocimiento:

```bash
nmap -sCV -p22,80 -n -Pn -vvv --min-rate 5000 -oN version 10.10.11.92
```

![Pasted image 20251102133823](<../../../.gitbook/assets/Pasted image 20251102133823.png>)

Podemos ver como en el servicio http se ha intentado hacer una redirección al dominio http://conversor.htp/ así que vamos a añadirlo al archivo /etc/hosts:

![Pasted image 20251102134348](<../../../.gitbook/assets/Pasted image 20251102134348.png>)

Me registro y accedo. Veo una sección de about con lo que parece ser, por un lado, un nombre de usuario, y por otro, un archivo descargable que parece ser un código fuente. Me lo descargo:
