tags:
_____
Para crackear la contraseña de un .zip en el equipo local, primero hay que extraer el hash utilizando la herramienta zip2john, con el siguiente comando:

```bash
zip2john archivo.zip > hash
```

Ahora obtenemos el archivo hash y podemos ahora crackear la contraseña:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash
```

>NOTA: Para obtener el hash de cada tipo de archivo, hay que pasarlo primero por la herramienta específica para cada archivo. Por ejemplo, **zip2john** o **keepass2john**.

Lista de las posibles opciones:

También se puede usar la herramienta **HashIdentifier** para determinar el hash de un archivo concreto y así poder indicárselo a **john**
Simplemente pegando el hash, ya nos dirá cuál es. Luego podemos indicarlo en john antes de ejecutar el comando:

```bash 
john --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt hash
```

