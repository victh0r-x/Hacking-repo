# COMANDO SED (Para sustituir letras o palabras por otros, de distinta extensión)

Por ejemplo vamos a sustituir la palabra Hola por la palabra Mario con el comando sed:  Incluso también podemos sustituir un espacio por lo que queramos, por ejemplo donde había un espacio ponemos un guión:  Lo que pasa que sólo nos sustituye con la primera coincidencia, por lo que si queremos hacer una sustitución en todo el documento debemos poner una 'g' al final:  También puedo eliminar o modificar el último elemento de una cadena de texto, escribiendo sed ‘s/.$//’  **Eliminar filas con sed** Podemos también eliminar filas con el comando sed, donde tenemos el siguiente texto:  Y ahora podemos eliminar la tercera file con sed '3d':  Aunque también podemos eliminar más de dos filas a la vez de la siguiente forma:  **Añadir texto al final de cada línea** También podemos añadir texto al final de cada línea de un archivo, por ejemplo vamos a suponer que al final de cada línea quiero añadir la frase 'El gato es feliz', pues lo haría de la siguiente manera:

```bash
cat texto.txt | sed -i 's/$/ El gato es feliz/' texto.txt
```
