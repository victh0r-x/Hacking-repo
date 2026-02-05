tags:
____
```bash
seq 1 254 | xargs -P 50 -I {} bash -c 'ping -c 1 -W 1 10.10.10.{} &> /dev/null && echo "[+] El host 10.10.10.{} - ACTIVO"'
```

Este oneliner es un equivalente a usar arp-scan, aunque es mucho mas ruidoso pero es muy útil cuando se vulnera una máquina que no tiene muchos recursos para utilizar.

```bash
seq 1 65535 | xargs -P 500 -I PORT bash -c 'timeout 1 bash -c "echo > /dev/tcp/10.10.10.128/PORT" 2>/dev/null && echo "[+] Puerto PORT - ABIERTO"'
```

Este oneliner sirve para ahcer un escaneo de puertos abiertos en una máquina cuando no tenemos nmap ni ninguna otra herramienta instalada.
