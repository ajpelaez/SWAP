# Pr�ctica 5 - Replicaci�n de bases de datos MySQL 
  
IP M�quina1: 192.168.1.51  
IP M�quina2: 192.168.1.52  
Sistema Operativo: Debian 8

**Creaci�n de la base de datos y inserci�n:**  
  
En primer lugar comenzaremos creando la bd y a�adiendo algunos datos en nuestra m�quina1:  
![alt](http://i.imgur.com/s8pWfEx.png)  
  
En la m�quina2 hacemos el mismo proceso pero sin insertar los datos, �nicamente creamos la base de datos.  
  
  
   
**Creaci�n de copia de seguridad de una base de datos usando mysqldump:**  
Crearemos la copia de seguridad de la base de datos contactos en la m�quina 1.  
Para ello nos conectamos en el mysql de nuestra m�quina 1 y lanzamos:  
FLUSH TABLES WITH READ LOCK;  
Nos salimos del mysql y ejecutamos el comando mysqldump:  
mysqldump contactos -p > /root/contactos.sql  
Y como comprobamos obtendremos un fichero sql con todos los datos que ten�amos almacenados.  
Ahora desbloqueamos las tablas entrando en mysql y lanzando:  
UNLOCK TABLES;  
  
  
   
**Restauraci�n de la copia de seguridad en la m�quina2:**  
En primer lugar copiamos el archivo sql creado en la m�quina 1 a la m�quina 2, lanzando el siguiente comando desde nuestra m�quina2:  
scp root@192.168.1.51/root/contactos.sql /root/  
Ahora ejecutamos el archivo sql:  
mysql -p contactos < /root/contactos.sql  
Y comprobamos que correctamente se han inserado los datos con un select a la tabla datos, de la base de datos contactos.  
  
  
   
**Configuraci�n maestro-esclavo para replicaci�n autom�tica de datos:**  
Primero comenzamos preparando el maestro, editamos el archivo de configuraci�n de mysql /etc/mysql/my.cnf:  
Comentamos:  
\#bind-address 127.0.0.1  
Y nos aseguramos de que estas l�neas esten sin comentar, si no estan las escribimos:  
log_error = /var/log/mysql/error.log  
server-id = 1  
log_bin = /var/log/mysql/bin.log  
Y reiniciamos: service mysql restart  
  
Ahora prepararemos el esclavo, igualmente editamos el archivo de configuracion de mysql con los mismos datos que anteriormente excepto:  
server-id = 2  
Reiniciamos el servicio mysql: service mysql restart  
  
Ahora crearemos el usuario esclavo en el servidor maestro:  
![alt](http://i.imgur.com/UdijAzo.png)  
  
Obtenemos los datos de nuestra base de datos para usarlos en la configuraci�n del esclavo:  
![alt](http://i.imgur.com/m0hdzeO.png)  

Una vez terminada la configuraci�n del maestro pasamos a configurar el esclavo:  
![alt](http://i.imgur.com/HFa03aD.png)  
Desbloqueamos las tablas en el maestro:  
UNLOCK TABLES;  
Y comprobamos que el esclavo este funcionando correctamente:  
SHOW SLAVE STATUS\G;
  
![alt](http://i.imgur.com/XVpHEyw.png)  
  
Como vemos la configuraci�n esta funcionando correctamente, ahora pasaremos a la pr�ctica, insertaremos datos en el maestro y veremos si los obtenemos en el esclavo:  
![alt](http://i.imgur.com/RWluM4H.png)  
  
  
  
  
**Configuraci�n maestro-maestro:**  
Empezamos por crear el usuario esclavo en nuestra m�quina2 igual que hicimos en la 1:  
![alt](http://i.imgur.com/UdijAzo.png)  
  
Ahora obtenemos los datos de la m�quina2 para usarlos en la configuraci�n del esclavo:  
![alt](http://i.imgur.com/7WBPCGF.png)  

Y pasamos a configurar el esclavo en la m�quina1:  
![alt](http://i.imgur.com/nhzkdRS.png)  

Desbloqueamos las tablas en la m�quina2:  
UNLOCK TABLES;  
Y comprobamos que el esclavo de la m�quina1 este funcionando correctamente:  
START SALVE;  
SHOW SLAVE STATUS\G;  
![alt](http://i.imgur.com/KP7AVOg.png)  
  
Todo esta funcionando correctamente ahora pasaremos a ver si nuestra configuraci�n maestro-maestro realmente funciona:  
![alt](http://i.imgur.com/4fS0vYl.png)  











