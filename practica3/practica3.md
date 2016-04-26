# Pr�ctica 3 - Balanceo de carga
 
IP Maquina1: 192.168.1.51  
IP Maquina2: 192.168.1.52  
IP Maquina3 (Balanceador): 192.168.1.53  
Ip Maquina para pruebas: 192.168.1.70  
Sistema Operativo: Debian 8  
  
**-Comenzamos preparando las m�quinas 1 y 2:**   
Desactivamos el crontab que ten�amos programado para que clonara el contenido de m1 en m2.  
Editamos el contenido de /var/www/html/index.html para que cada m�quina muestre:  
Hola soy la m�quina1  
Hola soy la m�quina2  
 
**-Ahora preparamos el balanceador (m�quina 3):**  
Empezamos por ver que no tenga ningun proceso funcionando en el puerto 80 con el comando:  
netstat -tulpn | grep :80  
En mi caso ten�a apache2 en este puerto por lo que paro el servicio:  
service apache2 stop  
Comprobamos que tenga conexi�n con las m�quinas 1 y 2 :  
![alt](http://i.imgur.com/9ijbUwI.png)  
Con el comando curl de paso tambi�n comprobamos que la configuraci�n que hicimos anteriormente esta funcionando correctamente pues al hacer curl a cada m�quina nos muestra el correspondiente Hola soy la m�quinaX. 
 
**-Instalaci�n de nginx como balanceador:**  
apt-get update 
apt-get install nginx 
Ahora editamos el archivo de configuraci�n de nginx:  
nano /etc/nginx/sites-available/default  
Y lo dejamos asi:  
![alt](http://i.imgur.com/9fHWua1.png)  
Reiniciamos nginx:  
service nginx restart  
Comprobamos que funciona desde otra m�quina conectada en la misma red:  
![alt](http://i.imgur.com/vhCwIqZ.png)  
Como vemos funciona correctamente, ahora asignameros a la m�quina1 weight=2 en el fichero de configuraci�n para que reparta el doble de carga sobre la m�quina1. Y comprobamos que funcione:  
![alt](http://i.imgur.com/12Ypeeg.png)  
  
  
**-Instalaci�n de haproxy como balanceador:**  
Para esto vamos a clonar la m�quina que usamos como balanceador anteriormente, lo primero que haremos una vez clonada sera parar nginx:  
service nginx stop  
Ahora empezamos a instalar haproxy:  
apt-get install haproxy  
Editamos el archivo de configuracion:  
nano /etc/haproxy/haproxy.cfg  
![alt](http://i.imgur.com/fP2ov7X.png)  
Ejecutamos haproxy:  
/usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg  
Y comprobamos que funcione correctamente:  
![alt](http://i.imgur.com/0AReyp1.png)  
Ahora editaremos el archivo de configuracion para repartir el doble de carga en la m�quina1 y volvemos a ejecutar haproxy con el comando anterior:  
![alt](http://i.imgur.com/ITaxyuZ.png)  
Y comprobamos que funcione:  
![alt](http://i.imgur.com/rbSDNuz.png)  

 
**-Instalaci�n de Zen load balancer:**  
Lo primero es descargar la iso, montar la m�quina virtual, instalarlo todo. Una vez hecho esto lo siguiente es configurar las interfaces desde el interfaz web para que vea a las m�quinas que sirven el contenido web.  
Accedemos a la interfaz web desde: https://192.168.0.110:444 (192.168.0.110 es la IP que le asignamos anteriormente en la instalaci�n).  
Entramos con admin:admin  y nos vamos a la secci�n settings -> interfaces.  
Una vez aqui configuramos la interfaz de nuestra red interna:  
![alt](http://i.imgur.com/BaRdXIG.png)  
Despu�s de esto comprobamos que podamos hacer ping desde nuestra m�quina balanceador a las otras dos.  
Una vez esta todo correcto, pasamos a crear la "Granja web".  
Nos vamos a manage -> farms y creamos una nueva http:  
![alt](http://i.imgur.com/e3M4gWu.png)  
Despu�s pasamos a configurarla dej�ndola asi:  
![alt](http://i.imgur.com/kckkJcV.png)  
A�adimos un nuevo servicio, al que tenemos que agregar los 2 servidores reales:  
![alt](http://i.imgur.com/4cMJnOj.png)  
Despu�s reiniciamos la granja y comprobamos que todo este funcionando correctamente:  
![alt](http://i.imgur.com/7Y2Ms8w.png)  
  
Como vemos el balanceador esta funcionando correctamente.  
 






