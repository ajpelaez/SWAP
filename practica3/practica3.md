# Práctica 3 - Balanceo de carga
 
IP Maquina1: 192.168.1.51  
IP Maquina2: 192.168.1.52  
IP Maquina3 (Balanceador): 192.168.1.53  
Ip Maquina para pruebas: 192.168.1.70  
Sistema Operativo: Debian 8  
  
**-Comenzamos preparando las máquinas 1 y 2:**   
Desactivamos el crontab que teníamos programado para que clonara el contenido de m1 en m2. 
Editamos el contenido de /var/www/html/index.html para que cada máquina muestre: 
Hola soy la máquina1 
Hola soy la máquina2 
 
**-Ahora preparamos el balanceador (máquina 3):**  
Empezamos por ver que no tenga ningun proceso funcionando en el puerto 80 con el comando: 
netstat -tulpn | grep :80 
En mi caso tenía apache2 en este puerto por lo que paro el servicio: 
service apache2 stop 
Comprobamos que tenga conexión con las máquinas 1 y 2 : 
![alt](http://i.imgur.com/9ijbUwI.png)  
Con el comando curl de paso también comprobamos que la configuración que hicimos anteriormente esta funcionando correctamente pues al hacer curl a cada máquina nos muestra el correspondiente Hola soy la máquinaX. 
 
**-Comenzamos con la instalación de nginx como balanceador:**  
apt-get update 
apt-get install nginx 
Ahora editamos el archivo de configuración de nginx: 
nano /etc/nginx/sites-available/default
Y lo dejamos asi: 
![alt](http://i.imgur.com/9fHWua1.png)  
Reiniciamos nginx: 
service nginx restart 
Comprobamos que funciona desde otra máquina conectada en la misma red: 
![alt](http://i.imgur.com/vhCwIqZ.png) 


 


 






