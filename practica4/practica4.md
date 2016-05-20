# Práctica 4 - Comprobar el rendimiento de servidores web  
  
IP Máquina1: 192.168.1.51  
IP Máquina2: 192.168.1.52  
IP Máquina3 (Balanceador): 192.168.1.53  
Ip Máquina para lanzar Benchmarks: 192.168.1.70  
Sistema Operativo: Debian 8

**Preparación de las máquinas:**  
  
Entramos en la máquina 1 y la máquina 2 y creamos el archivo prueba.html en /var/www/html/ con el siguiente contenido:  
~~~
<html>  
<head>  
<title>pruebas</title>  
</head>  
<body>  
archivo para realizar la prueba  
</body>  
</html>
~~~  
  
**[Apache Benchmark] Prueba a 1 servidor solo:**   
  
Entramos en la máquina de pruebas, y lanzamos el comando:  
ab -n 1000 -c 5 http://192.168.1.51/prueba.html > test.txt  
Asi los resultados del test se nos guardaran en test.txt, hacemos un nano o un cat a este archivo para verlos y los metemos en nuestra tabla excel, hasta completar 10 pruebas y generar un gráfico.  
  
![alt](http://i.imgur.com/Q5qvt48.png)  
  
**[Apache Benchmark] Prueba balanceador nginx:**  
   
Entramos en la máquina de pruebas, y lanzamos el comando:  
ab -n 1000 -c 5 http://192.168.1.53/prueba.html > test.txt  
Recopilamos los datos de 10 ejecuciones en la siguiente tabla:  
  
![alt](http://i.imgur.com/eMCNjiD.png)  
  
**[Apache Benchmark] Prueba balanceador haproxy:**
  
ab -n 1000 -c 5 http://192.168.1.53/prueba.html > test.txt  
Recopilamos los datos de 10 ejecuciones en la siguiente tabla:  
  
![alt](http://i.imgur.com/QAGTMTs.png)  

**[Apache Benchmark] Comparaciones:**  
  
![alt](http://i.imgur.com/1ukgbGV.png)  
  
![alt](http://i.imgur.com/TTBc5Nb.png)  
  
**[Siege] Prueba a 1 servidor solo:**   
  
Antes de nada instalamos siege:  
apt-get install siege  
Haremos las pruebas en 10 segundos, lanzamos el comando:  
siege -b -t10S -v http://192.168.1.51/prueba.html  
Recopilamos los datos de 10 ejecuciones en la siguiente tabla:  
  
![alt](http://i.imgur.com/2na0vqp.png)  
  
**[Siege] Prueba balanceador nginx:**   
  
siege -b -t10S -v http://192.168.1.53/prueba.html  
Recopilamos los datos de 10 ejecuciones en la siguiente tabla:  
  
![alt](http://i.imgur.com/LI2Ae99.png)  
  
**[Siege] Prueba balanceador haproxy:**   
  
siege -b -t10S -v http://192.168.1.53/prueba.html  
Recopilamos los datos de 10 ejecuciones en la siguiente tabla:  
  
![alt](http://i.imgur.com/fCJn35G.png)  
  
**[Siege] Comparaciones:**  
  
![alt](http://i.imgur.com/Uln8zp5.png)  
  
  
**[Httperf] Prueba a 1 servidor solo:**  
Primero instalamos httperf con:  
apt-get install httperf  
Ahora lanzaremos el comando para realizar la prueba:  
httperf --server=192.168.1.51 --uri=/prueba.html --timeout=10 --num-conns=2000  
Esto quiere decir que pediremos el archivo prueba.html de nuestro servidor, que enviaremos 2000 peticiones y que esperaremos 10 segundos si no recibimos respuesta del servidor.  La salida que nos da es la siguiente:  
![alt](http://i.imgur.com/U2Cm1YG.png)  
  
Recopilamos los datos de 10 ejecuciones en la siguiente tabla:  
![alt](http://i.imgur.com/pdx6yDo.png)  
  
**[Httperf] Prueba balanceador nginx:**  
httperf --server=192.168.1.53 --uri=/prueba.html --timeout=10 --num-conns=2000  
Recopilamos los datos de 10 ejecuciones en la siguiente tabla:  
![alt](http://i.imgur.com/J5iyoJE.png)  
  
**[Httperf] Prueba balanceador haproxy:**  
httperf --server=192.168.1.53 --uri=/prueba.html --timeout=10 --num-conns=2000  
Recopilamos los datos de 10 ejecuciones en la siguiente tabla:  
![alt](http://i.imgur.com/1e7i9kd.png)  
  
  
**[Httperf] Comparaciones:**  
![alt](http://img.prntscr.com/img?url=http://i.imgur.com/LeobCcO.png)  
  





