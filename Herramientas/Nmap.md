tags:
_________________________



Este es uno de los comandos más completos que podemos usar con nmap:

```bash
nmap -sSCV -p- --min-rate 5000 -n -Pn -vvv --open -oN scan 172.17.0.2
```

### 🔍 **Desglose de parámetros**
____________________________

| Parámetro             | Significado             | Explicación práctica                                                                                                            |
| --------------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **`nmap`**            | Network Mapper          | Herramienta para escaneo y descubrimiento de red.                                                                               |
| **`-sS`**             | SYN Scan (Stealth Scan) | Envía paquetes SYN sin completar la conexión TCP (rápido y sigiloso).                                                           |
| **`-sC`**             | Script Scan (Default)   | Ejecuta los scripts de la categoría “default” (`--script=default`) para obtener información extra de los servicios detectados.  |
| **`-sV`**             | Version Detection       | Intenta identificar la versión exacta del software que corre en cada puerto abierto.                                            |
| **`-p-`**             | All Ports               | Escanea los **65.535 puertos TCP**. Si no se especifica, Nmap solo escanea los 1.000 más comunes.                               |
| **`--min-rate 5000`** | Minimum Packet Rate     | Envía al menos 5.000 paquetes por segundo. Acelera el escaneo, pero puede generar ruido o paquetes perdidos si la red es lenta. |
| **`-n`**              | No DNS Resolution       | Evita que Nmap intente resolver nombres de dominio. Acelera el proceso y evita tráfico DNS innecesario.                         |
| **`-Pn`**             | Treat Host as Online    | Omite la fase de “host discovery” (ping). Útil si el host no responde a ICMP o si se sabe que está activo.                      |
| **`-vvv`**            | Very Verbose            | Muestra salida detallada con información adicional durante el proceso (triple verbose).                                         |
| **`--open`**          | Show Only Open Ports    | Filtra resultados para mostrar únicamente los puertos en estado **open**, ocultando los cerrados o filtrados.                   |
| **`-oN scan`**        | Output Normal           | Guarda la salida en un archivo de texto plano llamado `scan`. Facilita revisarlo más tarde o documentarlo.                      |
| **`172.17.0.2`**      | Target IP               | Dirección IP del objetivo a escanear.                                                                                           |
| **`-sN`**             |                         |                                                                                                                                 |
| **`-f`**              |                         |                                                                                                                                 |
| **`-sY`**             |                         |                                                                                                                                 |
| **``-sn``**           | Barrido de hosts        |                                                                                                                                 |

____________


