tags: #fuzzing 
_________________________________________________________
Gobuster es una potente herramienta diseñada para hacer fuzzing, es decir, para hacer un descubrimiento de diferentes parámetros de un servicio web, así como directorios, archivos, subdominios o rutas ocultas.

```bash
gobuster dir -u http://172.17.0.2/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x .php,.xml,.txt,.html -t 10 -o dirs.txt
```

>NOTA: A veces será necesario aplicar el parámetro --add-slash para que haga fuzzing correctamente.
