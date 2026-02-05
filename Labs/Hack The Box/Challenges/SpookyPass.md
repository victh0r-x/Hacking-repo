tags:
___
Una vez descargado el zip, lo extraemos y vemos que hay un archivo llamado **pass**, el cual vemos con el comando **file** que se trata de un binario ejecutable, así que lo ejecutamos y vemos lo siguiente:

![Pasted image 20251102062708.png](Pasted%20image%2020251102062708.png)

Para encontrar la contraseña, usamos el comando:

```bash
strings pass
```

Y encontramos lo siguiente:

![Pasted image 20251102062820.png](Pasted%20image%2020251102062820.png)

```text
s3cr3t_p455_f0r_gh05t5_4nd_gh0ul5
```

Ahora lo ejecutamos, accedemos, y ya obtenemos la flag.

