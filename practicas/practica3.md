# Práctica3: Balanceo de carga

En esta práctica instalaremos 2 balanceadores de carga para que distribuyan el tráfico entre nuestras 2 máquinas servidoras, e instalaremos un cliente para realizar unas pruebas con Apache Benchmarck para comprobar el funcionamiento de dichos balanceadores.

# Nginx

Tras instalar nginx (máquina con IP 192.168.56.30) e iniciar el servicio, procederemos a modificar el fichero de configuración /etc/nginx/conf.d/default.conf. Habrá que dejarlo de la siguiente forma, al menos por el momento:

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img10.png)

Tras guardar los cambios, hay que volver a reiniciar el servicio. Después, para probar que funciona la configuración habrá que hacer curl a los servidores para que nos muestren la página de inicio de cada una de ellas.

Se puede modificar la configuración del documento para aplicar cambios en el comportamiento de nginx durante el balanceo.

# Haproxy

Tras instalar haproxy (máquina con IP 192.168.56.40), procederemos a modificar el fichero de configuración /etc/haproxy/haproxy.cfg. Habrá que dejarlo de la siguiente forma, al menos por el momento:

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/img11.png)

Una vez salvada la configuración en el fichero, lanzamos el servicio haproxy mediante el comando _sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg_

Ahora solo es necesario probar que el servicio funciona correctamente haciendo curl hacia las máquinas servidoras como hicimos en el caso anterior con nginx.

También, al igual que pasaba con nginx podemos cambiar la configuración de haproxy a la hora de realizar el balanceo de carga.

# Pruebas con AB

Instalamos una nueva máquina para que realice peticiones mediante Apache Benchmarck a las máquinas servidoras. Para ello, simplemente ejecutamos en consola el comando ab -n 1000 -c 10 http://ip_maquina/index.html (la idea es obtener estadísticas de ambos balanceadores para "medir" su rendimiento, aunque en este caso no sea muy realista).

Con -n, estamos indicando el número de peticiones que se realizan, y con -c el grado de concurrencia de éstas.

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/nginx.png)

![comentario](https://github.com/MiguelJMartinez/SWAP/blob/master/imagenes/haproxy.png)
