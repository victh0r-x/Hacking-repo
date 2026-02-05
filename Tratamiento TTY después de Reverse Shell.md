_______________________________________
Una vez obtenemos una reverse shell necesitamos estar comodos con una **TTY** interactiva para evitar problemas, como **cerrar la conección** sin querer o simplemente para poder **desplazarnos** con las flechas, poder usar el **tab** para el auto completado de rutas, etc. Esto es muy sencillo de lograr, simplemente debemos de hacer un tratamiento de la tty siguiendo estos pasos:

Empezamos en la reverse shell obtenida

> NOTA: Las órdenes a ejecutar son solamente las que se encuentran en los recuadros azules.

```bash
script /dev/null -c bash
```

Luego de esto presionamos **ctrl_z** para suspender la shell

```bash
^Z
```

Ahora resetearemos la configuración de la shell que dejamos en segundo plano indicando **reset** y **xterm**

```bash
stty raw -echo; fg
```

```bash
reset xterm
```

Exportamos las variables de entorno **TERM** y **SHELL**

- `export TERM=xterm` -> Debemos hacer esto ya que a pesar de haberle indicado que queríamos una **xterm** al momento de reiniciarlo la variable de entorno **TERM** vale **dump** (Se usa esta variable para poder usar los atajos de teclado).
- `export SHELL=bash` -> Para que nuestra shell sea una bash.

```bash
export TERM=xterm
export SHELL=bash
```

Ya con esto hecho tendríamos una **TTY** full interactiva, pero falta una cosa para que sea lo más comoda posible, setear las filas y columnas, esto para poder ocupar toda nuestra pantalla ya que en este momento solo podemos usar una porción de esta

Y las seteamos en la reverse shell

```bash
stty rows 52 columns 189
```

Y listo!, ya podemos disfrutar de una shell totalmente interactiva y muy comoda para continuar rompiend