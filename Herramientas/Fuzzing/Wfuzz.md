tags: #fuzzing 
____


```bash
wfuzz -c -hc=404 -w DICCIONARIO -u http://IP_VICTIMA/FUZZ
```

>NOTA: La palabra FUZZ está reservada para este tipo de herramienta, y corresponde al lugar en el que se van a reemplazar cada uno de los directorios del diccionario que hemos indicado en el paso previo.

