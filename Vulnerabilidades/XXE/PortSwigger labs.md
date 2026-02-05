____
### Exploiting XXE using external entities to retrieve files
___
![Pasted image 20251024073859](../../Anexos/Pasted%20image%2020251024073859.png)

### Exploiting XXE to perform SSRF attacks
_____
![Pasted image 20251025035539](../../Anexos/Pasted%20image%2020251025035539.png)

Ahora vemos que en el output nos sale la palabra latest. Lo que haremos será colocarla en la URL como sub directorio, y nos irá arrojando cada vez un directorio distinto hasta que finalmente llegamos a extraer información privilegiada.

![Pasted image 20251025040651](../../Anexos/Pasted%20image%2020251025040651.png)

### Blind XXE with out-of-band interaction

