# Práctica 6 - Discos en RAID 
  
IP Máquina1: 192.168.1.51  (Actuara como servidor nfs)
IP Máquina1: 192.168.1.51  (Usará el espacio ofrecido por el servidor nfs)
Sistema Operativo: Debian 8

***

**Configuración de 2 discos en raid 1:**  
  
En primer lugar comenzaremos instalando mdadm:  
apt-get install mdadm  
Ahora apagaremos nuestra máquina y le añadiremos 2 discos virtuales:  
![alt](http://i.imgur.com/k2lvZ3y.png)  
Iniciamos nuestra máquina de nuevo y vemos la información acerca de los discos con:  
fdisk -l  
![alt](http://i.imgur.com/nEH8sSR.png)  
Como podemos ver los 2 discos de 5GB que agregamos son sdb y sdc.  
Ahora pasaremos a crear el raid 1:  
![alt](http://i.imgur.com/EjKetOn.png)  
Una vez creado le damos formato con:  
mkfs /dev/md0  
Y ahora pasamos a montarlo:  
mkdir /dat  
chmod 777 dat  
mount /dev/md0 /dat  
Comprobamos el estado del raid con:  
mdadm --detail /dev/md0  
![alt](http://i.imgur.com/sx2zdSq.png)  
Reiniciamos nuestra máquina.  
Ahora nos queda configurar el sistema para que monte el dispositivo creado al arrancar. Lo primero que haremos sera obtener la UUID del dispositivo que hemos creado:  
![alt](http://i.imgur.com/LidcYrP.png)  
Ya que tenemos el UUID pasamos a modificar el archivo /etc/fstab agregandole nuestro nuevo disco:  
![alt](http://i.imgur.com/WBAVOWG.png)  
  
***
  
**Simulación de un fallo en uno de los discos:**  
Crearemos unos cuantos ficheros en nuestro directorio en raid para ver si después de un fallo de disco podemos acceder a estos:  
![alt](http://i.imgur.com/LpRPOzs.png)  
Ahora que tenemos nuestro raid configurado y unos cuantos archivos almacenados simularemos un fallo en uno de los discos:  
![alt](http://i.imgur.com/4CeucPl.png)  
Ahora quitaremos el disco en caliente y meteremos otro "nuevo":  
![alt](http://i.imgur.com/fEr1hft.png)  
  
***  
  
**Configuración de un servidor nfs para servir nuestro raid:**  
Empezaremos instalando el servidor nfs en nuestra máquina1.  
apt-get install nfs-kernel-server  
Una vez instalado editaremos el archivo /etc/exports para que nuestra máquina2 pueda acceder al espacio que tenemos en raid:  
![alt](http://i.imgur.com/GIIlWRZ.png)  
Actualizamos la tabla de exportaciones:  
exportfs -a  
Y reiniciamos nuestro servidor nfs:  
service nfs-kernel-server restart  
  
Ya tenemos configurado nuestro servidor nfs, ahora pasaremos a configurar el cliente, instalamos el cliente nfs:  
apt-get install nfs-common  
Ahora creamos un directorio que servira como punto de montaje del directorio remoto:  
mkdir /remoto
Una vez hecho esto tenemos que editar el archivo /etc/fstab para agregar el directorio:  
![alt](http://i.imgur.com/yuiKweY.png)  
Y ahora montamos el sistema de archivos:  
mount /remoto  
Ahora comprobaremos que todo esta funcionando correctamente:  
![alt](http://i.imgur.com/rmxwnxr.png)  







