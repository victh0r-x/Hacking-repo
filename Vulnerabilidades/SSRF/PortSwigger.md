# PortSwigger

tags:

***

#### Basic SSRF against the local server

***

![900](<../../.gitbook/assets/Pasted image 20251025065944.png>)

Aquí vamos a aplicar un URL decode a ese enlace y probamos cambiar la ip por la del localhost:

![Pasted image 20251025070502](<../../.gitbook/assets/Pasted image 20251025070502.png>)

![Pasted image 20251025070536](<../../.gitbook/assets/Pasted image 20251025070536.png>)

Accedemos encontes a http://localhost/admin:

![Pasted image 20251025070632](<../../.gitbook/assets/Pasted image 20251025070632.png>)

Buscamos en el código el botón de delete:

Resuelto

#### Basic SSRF against another back-end system

***

![Pasted image 20251025071522](<../../.gitbook/assets/Pasted image 20251025071522.png>)

Para este laboratorio vamos a usar el intruder para fuzzear la parte del host de la IP.

![Pasted image 20251025073331](<../../.gitbook/assets/Pasted image 20251025073331.png>)

![Pasted image 20251025073435](<../../.gitbook/assets/Pasted image 20251025073435.png>)

![Pasted image 20251025073500](<../../.gitbook/assets/Pasted image 20251025073500.png>)

Resuelto

#### SSRF with blacklist-based input filter

***

![Pasted image 20251026060738](<../../.gitbook/assets/Pasted image 20251026060738.png>)

Para saltarse esta protección, se puede probar alguna de las siguientes opciones:

* Poner la ip del localhost, en vez de "localhost".
* La ip se puede representar de alguna de las siguientes formas:
  * 127.1
  * 7f.0.0.1
  * 0x7f000001
* En caso de detectar alguna que funcione, los subdirectorios se pueden representar también de distintas formas, como por ejemplo URL encodear la primera letra, o toda la palabra, pero teniendo en cuenta que también debemos URL encodear el % de la primera URL encode:

```bash
a = %61
% = %25
a, por tanto, vale = %2561
```

![Pasted image 20251026061305](<../../.gitbook/assets/Pasted image 20251026061305.png>)

En este caso, la opción que funciona es la siguiente:

![Pasted image 20251026061536](<../../.gitbook/assets/Pasted image 20251026061536.png>)

#### SSRF with filter bypass via open redirection vulnerability

***

![Pasted image 20251026064143](<../../.gitbook/assets/Pasted image 20251026064143.png>)
