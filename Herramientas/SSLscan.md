tags:
___
**sslscan** es una herramienta de auditoría de seguridad diseñada para analizar servicios que utilizan protocolos **SSL/TLS**. Su función principal es descubrir qué configuraciones de cifrado admite un servidor y determinar si es vulnerable a ataques conocidos.

### ¿Para qué sirve?
* **Identificar protocolos soportados:** Detecta si el servidor usa versiones obsoletas (SSLv2, SSLv3, TLS 1.0, TLS 1.1) o modernas (TLS 1.2, TLS 1.3).
* **Listar Cifrados (Ciphers):** Muestra qué algoritmos de cifrado acepta el servidor y cuáles prefiere.
* **Verificar Certificados:** Analiza la validez, el algoritmo de firma y la longitud de la clave del certificado digital.
* **Detectar Vulnerabilidades:** Avisa sobre debilidades como "Heartbleed" o el uso de certificados con firmas débiles.

---
### Comandos principales y más comunes

La sintaxis básica es: `sslscan [opciones] [servidor:puerto]`

| Comando | Descripción |
| :--- | :--- |
| `sslscan google.com` | El análisis **estándar**. Escanea todos los protocolos y cifrados en el puerto 443. |
| `sslscan --targets=lista.txt` | Escanea múltiples servidores definidos en un **archivo de texto**. |
| `sslscan --no-failed google.com` | Muestra **solo los cifrados aceptados** por el servidor (limpia el ruido). |
| `sslscan --tlsall google.com` | Fuerza el escaneo de **todas las versiones de TLS**. |
| `sslscan --show-certificate google.com` | Muestra la información detallada del **certificado SSL/TLS**. |
| `sslscan --xml=resultado.xml servidor` | Guarda el reporte en un **formato XML**. |
| `sslscan --ipv4` o `--ipv6` | Fuerza el escaneo a través de una versión específica de **protocolo IP**. |

---

> **Nota de seguridad:** Al ejecutar el comando, los resultados suelen aparecer en colores: el **rojo** indica configuraciones inseguras, el **amarillo** debilidades potenciales y el **verde** configuraciones seguras.