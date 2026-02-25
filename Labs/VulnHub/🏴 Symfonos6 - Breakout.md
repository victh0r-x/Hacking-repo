
tags:
___

# MAPA DE RED - PIVOTING
____
Primero vamos a escanear la red desde la máquina kali para buscar cuál es la máquina vulnerable dentro de mi red bridged. Para ello uso el comando arp-scan:

```bash
sudo arp-scan -I eth0 --localnet
```

![](Hacking-repo-obs/Anexos/Pasted%20image%2020260225180218.png)
# SYMFONOS 6

> **Plataforma:** Vulnhub | **Dificultad:** | **OS:** | **IP:**

---
## 🔍 Enumeración y Reconocimiento

Para comprobar que la máquina está encendida vamos a hacer un ping, usando el siguiente comando:

```bash
ping -c 1 192.168.0.40
```



Primero empezamos haciendo una enumeración completa de todos los puertos abiertos en la máquina. 

```bash
# nmap
nmap -sC -sV -oN scans/nmap_initial.txt <IP>
```

---
## 💥 Explotación

---
## 🔼 Escalada de Privilegios

---
## 🏁 Flags

|Flag|Hash|
|---|---|
|User|`...`|
|Root|`...`|

---
## 📝 Notas y Lecciones Aprendidas