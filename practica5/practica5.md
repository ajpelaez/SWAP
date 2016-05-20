# Práctica 5 - Replicación de bases de datos MySQL 
  
IP Máquina1: 192.168.1.51  
IP Máquina2: 192.168.1.52  
Sistema Operativo: Debian 8

**Creación de la base de datos y inserción:**  
  
En primer lugar comenzaremos creando la bd y añadiendo algunos datos en nuestra máquina1:  
![alt](http://i.imgur.com/s8pWfEx.png)  
  
En la máquina2 hacemos el mismo proceso pero sin insertar los datos, únicamente creamos la base de datos.  
  
  
   
**Creación de copia de seguridad de una base de datos usando mysqldump:**  
Crearemos la copia de seguridad de la base de datos contactos en la máquina 1.  
Para ello nos conectamos en el mysql de nuestra máquina 1 y lanzamos:  
FLUSH TABLES WITH READ LOCK;  
Nos salimos del mysql y ejecutamos el comando mysqldump:  
mysqldump contactos -p > /root/contactos.sql  
Y como comprobamos obtendremos un fichero sql con todos los datos que teníamos almacenados.  
Ahora desbloqueamos las tablas entrando en mysql y lanzando:  
UNLOCK TABLES;  
  
  
   
**Restauración de la copia de seguridad en la máquina2:**  
En primer lugar copiamos el archivo sql creado en la máquina 1 a la máquina 2, lanzando el siguiente comando desde nuestra máquina2:  
scp root@192.168.1.51/root/contactos.sql /root/  
Ahora ejecutamos el archivo sql:  
mysql -p contactos < /root/contactos.sql  
Y comprobamos que correctamente se han inserado los datos con un select a la tabla datos, de la base de datos contactos.  
  
  
   
**Configuración maestro-esclavo para replicación automática de datos:**  
Primero comenzamos preparando el maestro, editamos el archivo de configuración de mysql /etc/mysql/my.cnf:  
Comentamos:  
\#bind-address 127.0.0.1  
Y nos aseguramos de que estas líneas esten sin comentar, si no estan las escribimos:  
log_error = /var/log/mysql/error.log  
server-id = 1  
log_bin = /var/log/mysql/bin.log  
Y reiniciamos: service mysql restart  
  
Ahora prepararemos el esclavo, igualmente editamos el archivo de configuracion de mysql con los mismos datos que anteriormente excepto:  
server-id = 2  
Reiniciamos el servicio mysql: service mysql restart  
  
Ahora crearemos el usuario esclavo en el servidor maestro:  
![alt](http://i.imgur.com/UdijAzo.png)  
  
Obtenemos los datos de nuestra base de datos para usarlos en la configuración del esclavo:  
![alt](http://i.imgur.com/m0hdzeO.png)  

Una vez terminada la configuración del maestro pasamos a configurar el esclavo:  
![alt](http://i.imgur.com/HFa03aD.png)  
Desbloqueamos las tablas en el maestro:  
UNLOCK TABLES;  
Y comprobamos que el esclavo este funcionando correctamente:  
SHOW SLAVE STATUS\G;
  
![alt](http://i.imgur.com/XVpHEyw.png)  
  
Como vemos la configuración esta funcionando correctamente, ahora pasaremos a la práctica, insertaremos datos en el maestro y veremos si los obtenemos en el esclavo:  
![alt](http://i.imgur.com/RWluM4H.png)  
  
  
  
  
**Configuración maestro-maestro:**  
Empezamos por crear el usuario esclavo en nuestra máquina2 igual que hicimos en la 1:  
![alt](http://i.imgur.com/UdijAzo.png)  
  
Ahora obtenemos los datos de la máquina2 para usarlos en la configuración del esclavo:  
![alt](http://i.imgur.com/7WBPCGF.png)  

Y pasamos a configurar el esclavo en la máquina1:  
![alt](http://i.imgur.com/nhzkdRS.png)  

Desbloqueamos las tablas en la máquina2:  
UNLOCK TABLES;  
Y comprobamos que el esclavo de la máquina1 este funcionando correctamente:  
START SALVE;  
SHOW SLAVE STATUS\G;  
![alt](http://i.imgur.com/KP7AVOg.png)  
  
Todo esta funcionando correctamente ahora pasaremos a ver si nuestra configuración maestro-maestro realmente funciona:  
![alt](http://i.imgur.com/4fS0vYl.png)  











