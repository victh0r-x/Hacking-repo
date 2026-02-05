_____
### Fuerza bruta a paneles de login
____

```bash 
hydra -l admin -P /usr/share/wordlists/rockyou.txt "http-post-form://172.17.0.2:8080/login:username=admin&password=^PASS^:Invalid username or password"
```

Parámetros:

- **hydra** — la herramienta que realiza ataques por fuerza bruta/credential stuffing contra distintos servicios.
- **-t 64** — número de tareas (threads) concurrentes que Hydra arrancará; más = más velocidad pero más carga.
- **-l admin** — usa un **solo** usuario fijo llamado `admin` (opción `-L` sería para un fichero de usuarios).
- **-P <diccionario> — ruta al fichero de contraseñas (wordlist) que Hydra probará una por una.
- **<ip>** — dirección del host objetivo donde se enviarán las peticiones (puede incluir `:puerto` si no es 80).
- **http-post-form** — módulo/protocolo de Hydra que prueba formularios HTTP vía método POST.
- **"/my_weblog/admin.php:...:..."** — formato del módulo: `RUTA:POST_DATA:FAIL_STRING` (tres partes separadas por `:`).
- **/my_weblog/admin.php** — **ruta** en el servidor al endpoint del formulario (path relativo tras el host).
- **username=admin&password=^PASS^** — **cuerpo POST** que se enviará; `^PASS^` es el marcador que Hydra sustituye por cada contraseña candidata.
- **Incorrect** — cadena que indica fallo de autenticación; si no aparece, Hydra asumirá login exitoso.