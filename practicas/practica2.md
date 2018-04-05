# Práctica2: Clonar la información de in sitio web

Se ha instalado la herramienta rsync, que nos va a permitir clonar carpetas entre las distintas máquinas. Para probar su funcionamiento, y tras permitir que el usuario, en este caso migueljmartnez, sea el dueño de la carpeta a clonar (en este caso la carpeta va a ser /var/www), procedemos a clonar la carpeta de la máquina 1 en la máquina 2. Para ver que funciona la sincronización, se han creado los documentos hola.txt y hola2.txt en la máquina 1. Tras ejecutar el comando, podemos observar que ahora en la máquina 2 también aparece dicho documento en la ruta /var/www.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img4.png)

Tras hacerse la copia, comprobamos que el nuevo contenido de la carpeta /var/www en la máquina 2 coincide con el de la máquina 1:

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img5.png)

Se pueden añadir argumentos a rsync para que no copie determinados ficheros o subcarpetas dentro de la que deseamos clonar.

Ahora vamos a usar ssh-keygen para generar un par de claves pública-privada entre ambas máquinas. En la máquina 2 lanzamos el comando ssh-keygen -b 4096 -t rsa para que las produzca. Tras esto, hay que enviarle la clave al equipo principal (máquina 1). Para hacer esto, ejecutamos el comando ssh-copy-id 192.168.56.20 en la máquina 2. De esta forma, al hacer ssh desde la máquina 2 hasta la máquina 1 no será necesario introducir ninguna contraseña.


![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img6.png)

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img7.png)

Por último, mediante el uso de cron, vamos a programar una tarea en la máquina 2 para que se ejecute cada minuto. Para hacerlo, modificaremos el fichero /etc/crontab añadiendo la siguienter línea:

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img8.png)

De esta forma, cada minuto se realiza el clonado de la carpeta /var/www de la máquina 1 en la máquina 2.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img9.png)

