tags: #wordpress
_____
Sabremos que podemos abusar de este archivo, cuando accediendo a la URL vemos lo siguiente:

![Pasted image 20251013120703](Anexos/Pasted%20image%2020251013120703.png)

Si vemos esto, lo que debemos hacer ahora es enviar por POST un archivo malicioso .xml, y para ello vamos a crearlo con el nombre que deseemos y el siguiente código:

```php
POST /xmlrpc.php HTTP/1.1
Host: example.com
Content-Length: 135

<?xml version="1.0" encoding="utf-8"?> 
<methodCall> 
<methodName>system.listMethods</methodName> 
<params></params> 
</methodCall>
```

Ahora ejecutamos el comando:

```bash
curl -X POST 'IP_VICTIMA/xmlrpc.php' -d@archivo.xml
```

Este debería de ser el resultado:



