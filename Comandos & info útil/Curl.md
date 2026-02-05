`-0`, `--http1.0` — Usa HTTP/1.0.  
`-1`, `--tlsv1` — Forzar TLS v1.0 (antiguo).  
`-2`, `--sslv2` — Usar SSLv2 (obsoleto).  
`-3`, `--sslv3` — Usar SSLv3 (obsoleto/inseguro).  
`-4`, `--ipv4` — Forzar uso de IPv4.  
`-6`, `--ipv6` — Forzar uso de IPv6.  
`-A`, `--user-agent` `<string>` — Establecer User-Agent.  
`-a`, `--append` — Adjuntar al archivo en transferencias FTP.  
`-b`, `--cookie` `<data|file>` — Enviar cookie(s) o leer desde archivo.  
`-B`, `--use-ascii` — Forzar modo ASCII en FTP.  
`-c`, `--cookie-jar` `<file>` — Guardar cookies en archivo después de la operación.  
`-C`, `--continue-at` `<offset>` — Reanudar transferencia desde un offset (con `-C -` auto-detecta).  
`-d`, `--data` `<data>` — Enviar datos en POST (application/x-www-form-urlencoded).  
`--data-ascii` `<data>` — Igual que `--data` pero con ASCII.  
`--data-binary` `<data>` — Enviar datos binarios exactamente.  
`--data-raw` `<data>` — Similar a `--data`, sin interpretación de `@`.  
`--data-urlencode` `<data>` — Codificar y enviar como URL-encoded.  
`-D`, `--dump-header` `<file>` — Volcar cabeceras de respuesta a archivo.  
`--delegation` `<LEVEL>` — Control de delegación GSS-API.  
`--digest` — Usar HTTP Digest Authentication.  
`--disable-eprt` — Desactivar EPRT en FTP (modo extendido).  
`--disable-epsv` — Desactivar EPSV en FTP.  
`-e`, `--referer` `<URL>` — Establecer cabecera Referer.  
`--egdsocket` `<path>` — Usar socket EGD para entropy (TLS).  
`-E`, `--cert` `<cert[:passwd]>` — Usar certificado cliente (PEM).  
`--cert-type` `<type>` — Tipo de certificado (PEM/DER).  
`--key` `<key>` — Usar clave privada del certificado.  
`--key-type` `<type>` — Tipo de la clave.  
`--engine` `<name>` — Usar engine criptográfico (OpenSSL engine).  
`-f`, `--fail` — Fallar silenciosamente en HTTP codes >= 400 (salida ≠ 0).  
`-F`, `--form` `<name=content>` — Enviar multipart/form-data (formularios/archivos).  
`--ftp-account` `<data>` — Mandar comando ACCT tras login FTP.  
`--ftp-alternative-to-user` `<cmd>` — Comando alternativo al USER en FTP.  
`--ftp-create-dirs` — Crear directorios remotos si no existen (FTP).  
`--ftp-method` `<type>` — Seleccionar método FTP (multicwd, singlecwd, nocwd).  
`--ftp-pasv` — Forzar modo pasivo en FTP.  
`--ftp-skip-pasv-ip` — Ignorar IP devuelta por PASV (útil con NAT).  
`--ftp-ssl` — Solicitar SSL para FTP (implicit/explicit según build).  
`--ftp-ssl-reqd` — Requerir SSL en FTP.  
`--ftp-ssl-control` — Proteger canal de control FTP con SSL.  
`-G`, `--get` — Poner datos en la URL como query string (con `-d`).  
`-g`, `--globoff` — Desactivar globbing de URL.  
`-H`, `--header` `<header>` — Añadir cabecera HTTP personalizada.  
`-h`, `--help` — Mostrar ayuda y salir.  
`-I`, `--head` — Hacer petición HEAD y mostrar sólo cabeceras.  
`-i`, `--include` — Incluir cabeceras en la salida junto al cuerpo.  
`--interface` `<name|addr>` — Forzar interfaz de red o dirección local.  
`--ipv4` — Igual que `-4` (forzar IPv4).  
`--ipv6` — Igual que `-6` (forzar IPv6).  
`-j`, `--junk-session-cookies` — Ignorar cookies de sesión guardadas.  
`-J`, `--remote-header-name` — Usar nombre de `Content-Disposition` al usar `-O`.  
`-k`, `--insecure` — No verificar certificado SSL/TLS del servidor.  
`-K`, `--config` `<file>` — Leer opciones desde archivo de configuración.  
`-l`, `--list-only` — FTP: listar únicamente nombres de archivos.  
`-L`, `--location` — Seguir redirecciones (301/302/3xx).  
`--location-trusted` — Seguir redirecciones y reenviar credenciales.  
`-m`, `--max-time` `<seconds>` — Tiempo máximo total de la operación.  
`--max-filesize` `<bytes>` — Rechazar descargas mayores que X.  
`--mail-from` `<address>` — SMTP: dirección From.  
`--mail-rcpt` `<address>` — SMTP: añadir destinatario.  
`--mail-auth` `<address>` — SMTP: dirección para AUTH.  
`--mail-ssl` — Usar SSL en SMTP.  
`--mail-ssl-reqd` — Requerir SSL en SMTP.  
`--metalink` — Procesar Metalink (descarga múltiple).  
`--negotiate` — Usar Negotiate (SPNEGO / GSSAPI) para auth HTTP.  
`-n`, `--netrc` — Leer credenciales de `~/.netrc`.  
`--netrc-file` `<file>` — Usar archivo `.netrc` específico.  
`--netrc-optional` — Usar `.netrc` si existe (no obligatorio).  
`--no-buffer` — Desactivar buffer de salida (flujo inmediato).  
`--no-keepalive` — Desactivar keepalive TCP.  
`--no-sessionid` — TLS: desactivar reuse de session id.  
`--noproxy` `<no-proxy-list>` — No usar proxy para hosts listados.  
`-o`, `--output` `<file>` — Guardar salida en archivo local con nombre elegido.  
`-O`, `--remote-name` — Guardar con nombre remoto indicado en URL.  
`-p`, `--proxytunnel` — Usar túnel HTTP proxy (CONNECT).  
`--pubkey` `<key>` — SSH public key (en builds con libssh).  
`--proxy` `<[protocol://]host[:port]>` — Usar proxy (http/socks).  
`--proxy-anyauth` — Probar cualquier método de auth con proxy.  
`--proxy-basic` — Forzar Basic auth para proxy.  
`--proxy-digest` — Forzar Digest auth para proxy.  
`--proxy-header` `<header>` — Enviar cabecera al proxy.  
`--proxy-negotiate` — Proxy: usar Negotiate auth.  
`--proxy-user` `<user:password>` — Credenciales para proxy.  
`--pubkey` — (repetido en algunas builds; ver man).  
`-P`, `--ftp-port` `<addr>` — FTP: especificar dirección para active mode.  
`--pinnedpubkey` `<hashes>` — TLS: verificar que el pubkey coincida (pin).  
`--post301` — Con `-X POST`, reenviar POST tras 301 (no estándar).  
`--post302` — Igual para 302.  
`--post303` — Igual para 303.  
`--proto` `<protocols>` — Habilitar protocolos específicos (ej. `http,https`).  
`--proto-redir` `<protocols>` — Protocolos permitidos en redirecciones.  
`--proxy-tls` — Forzar TLS en proxy.  
`--proxy-cert` / `--proxy-key` — Certificado/clave para proxy TLS.  
`--pass` `<phrase>` — Contraseña para clave privada (varía según build).  
`-q` — Ignorar `~/.curlrc` y salir si usado como primer argumento.  
`-Q`, `--quote` `<cmd>` — FTP: ejecutar comandos tras conectar.  
`--random-file` `<file>` — Usar archivo como fuente de entropy (OpenSSL).  
`--raw` — No usar compresión HTTP automática, pasar datos sin modificar.  
`-r`, `--range` `<range>` — Descargar rango de bytes (HTTP/FTP).  
`-R`, `--remote-time` — Guardar tiempo de archivo remoto (mtime).  
`--resolve` `<host:port:addr>` — Resolver host localmente a IP (saltar DNS).  
`--retry` `<num>` — Reintentos en fallos temporales.  
`--retry-delay` `<seconds>` — Espera fija entre reintentos.  
`--retry-max-time` `<seconds>` — Tiempo máximo para reintentos.  
`--sasl-ir` — Enviar SASL initial response en auth.  
`-s`, `--silent` — Silenciar progreso y mensajes.  
`-S`, `--show-error` — Mostrar errores aun con `-s`.  
`--service-name` `<name>` — GSSAPI service name para Negotiate.  
`--ssl` — Forzar SSL/TLS (antiguo/depende de protocolo).  
`--ssl-allow-beast` — Permitir ataque BEAST mitigación (compatibilidad).  
`--ssl-no-revoke` — No comprobar revocación (Windows).  
`--ssl-reqd` — Requerir SSL/TLS (FTP/SMTP según build).  
`--ssl-allow-beast` — (repetido en algunas versiones).  
`--socks4` `<host[:port]>` — Usar proxy SOCKS4.  
`--socks4a` `<host[:port]>` — Usar SOCKS4a.  
`--socks5` `<host[:port]>` — Usar SOCKS5.  
`--socks5-hostname` `<host[:port]>` — SOCKS5 y resolver hostname remotamente.  
`--stderr` `<file>` — Redirigir stderr a archivo.  
`--tcp-nodelay` — Activar TCP_NODELAY (desactivar Nagle).  
`--tftp-blksize` `<value>` — TFTP blocksize.  
`--trace` `<file>` — Registrar trace detallado a archivo.  
`--trace-ascii` `<file>` — Trace en ASCII.  
`--trace-time` — Incluir timestamps en trace.  
`-T`, `--upload-file` `<file>` — Subir archivo (PUT/FTP).  
`--tlsv1.0`/`--tlsv1.1`/`--tlsv1.2`/`--tlsv1.3` — Forzar versión TLS específica.  
`--tls-max` `<VERSION>` — Versión TLS máxima permitida.  
`--tls13-ciphers` `<list>` — Cifras TLS1.3 (OpenSSL 1.1.1+).  
`--typed-placeholder` `<name:type>` — Placeholder tipado para URLs.  
`-u`, `--user` `<user:password>` — Credenciales para HTTP, FTP, etc.  
`-U`, `--proxy-user` `<user:password>` — Credenciales para proxy (alias).  
`--url` `<URL>` — Especificar URL; permite múltiples `--url`.  
`-v`, `--verbose` — Modo verbose: mostrar detalles de comunicación.  
`-V`, `--version` — Mostrar versión de curl y librerías y salir.  
`-w`, `--write-out` `<format>` — Escribir variables de salida (formato).  
`-x`, `--proxy` `<proxyhost[:port]>` — Alias para `--proxy`.  
`-X`, `--request` `<command>` — Forzar método HTTP (GET/POST/PUT/DELETE...).  
`-y`, `--speed-time` `<time>` — Tiempo para medir velocidad limite.  
`-Y`, `--speed-limit` `<rate>` — Límite de velocidad (bytes por segundo) para abortar.  
`--upload-file` — Igual que `-T`.  
`--url` — (repetido) especificar URL.  
`--user-agent` — (alias de `-A`).  
`--verbose` — (alias de `-v`).