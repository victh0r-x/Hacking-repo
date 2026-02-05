tags: 
______

```bash
smbmap -u 'usuario' -p 'contraseña' -H IP_VICTIMA
```

Para entrar en un recurso compartido, añadir el parámetro **-r** seguido del nombre del recurso, por ejemplo:

```bash
smbmap -u 'usuario' -p 'contraseña' -H IP_VICTIMA -r victor

```

```bash
smbmap -H IP_VICTIMA
```

```BASH
crackmapexec smb IP -u '' -p '' --shares --users
```

```bash
enum4linux IP 
```

```BASH
smbclient //IP/recurso -N
```

>NOTA: Probar smbmap y crackmapexec para probar usuarios y contraseñas
>Usar smbclient 

```bash
smbclient //172.17.0.2/macarena -U 'macarena%donald' -p 445 -c 'get "user.txt"' user.txt
```

Para entrar en múltiples directorios dentro de un recurso compartido, usar el siguiente comando:

```bash
smbmap -H 10.10.10.128 -r anonymous/backups --download log.txt
```