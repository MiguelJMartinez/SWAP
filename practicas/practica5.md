# Práctica 5: Replicación de bases de datos MySQL

En esta práctica vamos a realizar una configuración de una base de datos sencila en nuestra granja web.

# Crear una BD con al menos una tabla y algunos datos

Comenzamos en la máquina 1 (cuya ip es 192.168.56.10), creando una base de datos desde mysql llamada contactos. Una vez seleccionada, creamos la tabla de datos que tendrá un nombre y un teléfono, y le insertamos una tupla.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/db.png)


# Hacer una copia de seguridad de la BD usando mysqldump en la máquina principal y copiar el archivo de seguridad a la máquina secundaria

Ahora tenemos que crear una copia de seguridad. Para ello, primero bloqueamos la base de datos para que no se altere su configuración ni contenido durante el proceso. Tras realizarla, podremos desbloquearla.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/flush.png)

Vamos a realizar la copia de seguridad del archivo "contactosbd.sql" a la máquina secundaria.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/copia.png)

# Restaurar la copia de seguridad en la máquina secundaria (de forma manual) para que ambas máquinas tengan la misma BD

Para restaurar la base de datos que hemos exportado, tenemos que crearla a mano (como hemos hecho en la máquina principal). Después, con el comando  _mysql -u root -p contactos < /tmo/contactosbd.sql_ conseguimos hacer el volcado de las tablas en la base de datos creada.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/volcado.png)

# Configuración maestro-esclavo de los servidores MySQL para hacer la clonación de forma automática

Desde la máquina principal, editamos el fichero _/etc/mysql/mysql.conf.d/mysqld.cnf_
Hay que hacer las siguientes modificaciones:
1. Comentar _bind-address 127.0.0.1_
2. Descomentar _log_error = /var/log/mysql/error.log , server-id = 1 y log_bin = /var/log/mysql/mysql-bin.log_

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/mysql1.png)

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/mysql2.png)

Tras guardar el fichero reiniciamos el servicio mysql.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/restart.png)

Nos vamos a la máquina secundario y repetimos el mismo proceso (hay que comprobar que el server_id de la segunda máquina es distinto).

Volvemos a la máquina principal (que va a hacer el papel de maestro), y creamos un usuario al que le daremos permisos de acceso a la replicación de la base de datos.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/master.png)

Vamos a la máquina secundaria (que va a hacer de esclavo) y lanzamos el siguiente comando para vincular el esclavo al maestro:
_CHANGE MASTER TO MASTER_HOST='192.168.56.10', MASTER_USER='esclavo', MASTER_PASSWORD='esclavo', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=694, MASTER_PORT=3306;_

Volvemos al maestro y activamos las tablas con _unlock tables;_

Para comprobar que todo funciona correctamente nos vamos al esclavo y ejecutamos la orden _start slave;_. Si queremos ver posibles errores lanzamos el comando _show slave status\G_.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/error.png)

Como se puede apreciar, en este caso nos ha dado un error por los UUID's de las máquinas. Para solucionarlo basta con eliminar el fichero _/var/lib/mysql/auto.cnf_ y reiniciar el servicio mysql. Tras hacer esto, volvemos a comprobar que todo funciona como debe. Vemos que al insertar una nueva tupla en el maestro, podemos ver las tablas en el esclavo.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/bueno.png)
