# Pr�ctica2: Clonar la informaci�n de in sitio web

Se ha instalado la herramienta rsync, que nos va a permitir clonar carpetas entre las distintas m�quinas. Para probar su funcionamiento, y tras permitir que el usuario, en este caso migueljmartnez, sea el due�o de la carpeta a clonar (en este caso la carpeta va a ser /var/www), procedemos a clonar la carpeta de la m�quina 1 en la m�quina 2. Para ver que funciona la sincronizaci�n, se han creado los documentos hola.txt y hola2.txt en la m�quina 1. Tras ejecutar el comando, podemos observar que ahora en la m�quina 2 tambi�n aparece dicho documento en la ruta /var/www.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img4.png)

Tras hacerse la copia, comprobamos que el nuevo contenido de la carpeta /var/www en la m�quina 2 coincide con el de la m�quina 1:

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img5.png)

Se pueden a�adir argumentos a rsync para que no copie determinados ficheros o subcarpetas dentro de la que deseamos clonar.

Ahora vamos a usar ssh-keygen para generar un par de claves p�blica-privada entre ambas m�quinas. En la m�quina 2 lanzamos el comando ssh-keygen -b 4096 -t rsa para que las produzca. Tras esto, hay que enviarle la clave al equipo principal (m�quina 1). Para hacer esto, ejecutamos el comando ssh-copy-id 192.168.56.20 en la m�quina 2. De esta forma, al hacer ssh desde la m�quina 2 hasta la m�quina 1 no ser� necesario introducir ninguna contrase�a.


![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img6.png)

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img7.png)

Por �ltimo, mediante el uso de cron, vamos a programar una tarea en la m�quina 2 para que se ejecute cada minuto. Para hacerlo, modificaremos el fichero /etc/crontab a�adiendo la siguienter l�nea:

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img8.png)

De esta forma, cada minuto se realiza el clonado de la carpeta /var/www de la m�quina 1 en la m�quina 2.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img9.png)

