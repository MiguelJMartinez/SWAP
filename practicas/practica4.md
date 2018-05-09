# Práctica 4: Asegurar la granja web

En esta práctica vamos a realizar una configuración de seguridad de nuestra granja web.

# Instalar un certificado SSL autofirmado para configurar el acceso por HTTPS

Este certificado va a permitirnos autenticar y dar fiabilidad a nuestro sitio web de cara a los posibles clientes.

Para generarlo, solo debemos activar el módulo SSL de Apache, generar los certificados y especificarle la ruta a los certificados en la configuración. Ejecutamos como root los siguientes comandos:

a) a2enmod ssl

b) service apache2 restart (reiniciamos el servicio de apache)

c) mkdir /etc/apache2/ssl (si no teníamos previamente el directorio creado)

d) openssl req -x509 -nodes -days 365 -newkey rsa:2048-keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt

Tras ejecutar este último comando, nos pedirá una serie de datos para configurar el dominio.

Después, hay que editar el archivo de configuración /etc/apache2/sites-available/default-ssl, añadiendo las reglas para que localice los 2 ficheros anteriores (apache.crt y apache.key).

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/ssl.png)

Tras guardar los cambios, activamos ssl ejecutando sudo a2ensite default-ssl

Reiniciamos el servicio apache, y comprobamos que podemos hacer una peticion por https.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/curl.png)

Ahora tenemos que copiar el certificado desde la máquina ya configurada hasta el balanceador y el otro servidor. Para ello, desde las máquinas sin configurar ejecutamos el comando

sudo scp -r usuario@ip_maquina_configurada:/etc/apache2/ssl/ /etc/apache2/ssl

En el caso del balanceador nginx, crearemos la carpeta ssl como root y probaremos a hacer peticiones https a cada una de las máquinas servidoras.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/https.png)

Solo queda configurar nginx para que los clientes pueda realizar peticiones https. Nos vamos al archivo de configuracion /etc/nginx/conf.d/default.conf y lo dejamos como sigue:

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/conf.png)

Vemos que funciona

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/cliente.png)

# Configuración del cortafuegos con iptables

Nos creamos un script en una de las máquinas servidoras con las reglas necesarias para que se ejecute en el arranque del sistema.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/script.png)

Lo enviamos a /etc/init.d y añadimos una regla en /etc/crontab paraa que se ejecute el script cada vez que se reinicia el equipo:

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/crontab.png)

Comprobamos que sigue siendo posible hacer las peticiones por https, y con netstat -tulpn vemos los puertos que hay abiertos.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/net.png)
