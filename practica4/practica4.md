# Práctica 4 - Comprobar el rendimiento de servidores web  
  
IP Maquina1: 192.168.1.51  
IP Maquina2: 192.168.1.52  
IP Maquina3 (Balanceador): 192.168.1.53  
Ip Maquina para lanzar Benchmarks: 192.168.1.70  
Sistema Operativo: Debian 8

**Preparación de las máquinas:**  
Entramos en la máquina 1 y la máquina 2 y creamos el archivo prueba.html en /var/www/html/ con el siguiente contenido:  
	<html>  
	<head>  
	<title>pruebas</title>  
	</head>  
	<body>  
	archivo para realizar la prueba  
	</body>  
	</html>  
  
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
  

 






