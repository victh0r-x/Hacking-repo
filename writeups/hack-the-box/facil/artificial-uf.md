# Artificial - UF

tags: #linux #facil

***

Comenzamos haciendo un escaneo básico de puertos, para conocer cuales están abiertos y buscar un vector de ataque. Para ello usamos el siguiente comando:

```bash
nmap -sS -p- --min-rate 5000 -n -Pn -vvv -oN ports 10.10.11.74
```

Esto nos permite exportar al fichero **ports** todos los puertos en formato nmap. Obtenemos lo siguiente:

![Pasted image 20251026070557](<../../../.gitbook/assets/Pasted image 20251026070557.png>)

Ahora, vamos a lanzar el siguiente comando para averiguar cuál es la versión del servicio que corre por el puerto 80 y también lanzar unos scripts de nmap parra aplicar un reconocimiento:

```bash
nmap -sCV --min-rate 5000 -vvv -n -Pn -p22,80,8000 -vvv -oN version 10.10.11.74 
```

![Pasted image 20251026070709](<../../../.gitbook/assets/Pasted image 20251026070709.png>)

Además de ver abiertos los puertos 22, 80 y 8000, vemos que en el puerto 80 se ha fallado un intento de redirección al dominio http://artificial.htb/ así que vamos a añadirlo al archivo **/etc/hosts**:

![Pasted image 20251026071145](<../../../.gitbook/assets/Pasted image 20251026071145.png>)

![Pasted image 20251026071328](<../../../.gitbook/assets/Pasted image 20251026071328.png>)

Vemos un panel de registro, así que vamos a crearnos una cuenta para ver que se halla en el interior del servicio:

![Pasted image 20251026071454](<../../../.gitbook/assets/Pasted image 20251026071454.png>)

Al ver esto, vamos a intentar subir un archivo php con un código malicioso y ver qué pasa:
