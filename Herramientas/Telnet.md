tags:
____
# Comandos básicos SMTP para interactuar

Una vez conectado, puedes escribir comandos SMTP para simular el envío de un correo o verificar el servidor:

- **HELO**: Saluda al servidor (puedes usar tu dominio o cualquier nombre)

`HELO midominio.com`

- **MAIL FROM**: Define el remitente del correo

`MAIL FROM:<tu_email@midominio.com>`

- **RCPT TO**: Define el destinatario del correo

`RCPT TO:<destinatario@dominio.com>`

- **DATA**: Inicia la escritura del cuerpo del mensaje

`DATA`

Luego escribe el mensaje:

`Asunto: Prueba SMTP mediante Telnet Este mensaje es una prueba con Telnet en puerto 25. .`

_El punto solo en una línea indica el fin de mensaje._

- **QUIT**: Cierra la sesión con el servidor SMTP

`QUIT`

---