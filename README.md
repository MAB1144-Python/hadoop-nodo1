# VIDEO GUIDES
## Video guide and context
https://youtu.be/6mEEPnq_jng

## Download nodo1
https://drive.google.com/drive/folders/1TNJAykiBk83nlxib0DK9VvkRPyAtGDMA?usp=drive_link

## Problem solving
https://youtu.be/Z5MV6wJPAF4

## Other guides
https://www.youtube.com/watch?v=woUzV_liwto&ab_channel=AuriboxTraining

# hadoop-nodo1
hadoop-nodo1
Seguir los siguientes pasos de ejemplo para la ejecución de una aplicación de MapReduce en Hadoop

# contraseña para ingresar a inicio: bigdata1
# contraseña usuario root: bigdata2
# contraseña super usuario su: bigdata1

# Yarn se encarga de gestionar los recursos el cluster

# Montar nuestro primer cluster con yarn con la versión 2 de HDFS o Yarn
```
export HADOOP_HOME_WARN_SUPPRESS=1
export HADOOP_ROOT_LOGGER="WARN,DRFA"
```

# Vamos a lanzar una aplicación en un nodo de HDFS y Yarn, por ello si tenemos alguna ejecutandose detenerla
```
stop-dfs.sh
```

# Nos dirigimos al directorio donde está alojado nuestro hadoop
```
cd /opt/hadoop/etc/hadoop/
cp mapred-site.xml.template mapred-site.xml
```

# Ajustar el ssh
```
ssh localhost 
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa 
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 
chmod 0600 ~/.ssh/authorized_keys
```

# Formatear  file system
```
export PDSH_RCMD_TYPE=ssh
```
# si no arraca verificamos el servidor de hadoop
```
sudo service hadoop-hdfs-namenode status

sudo service hadoop-hdfs-namenode start
```
# Paso : Reiniciar Servicios
```
sudo service hadoop-hdfs-namenode restart
sudo service hadoop-hdfs-datanode restart
sudo service hadoop-yarn-resourcemanager restart
sudo service hadoop-yarn-nodemanager restart
```

# Primero debemos inciar el cluster luego el yarn
```
start-dfs.sh
start-yarn.sh
```

# Ver la ejecución de Hadoop
URL: http://localhost:50070/

# Posterior a este preambulo podemos empezar nuestro ejemplo
# Adaptar la instrucción del ejemplo y usarla en el documento asignado
```
export HADOOP_CLASSPATH=$(hadoop classpath)
echo $HADOOP_CLASSPATH


hadoop fs -mkdir /WordCountTutorial

hadoop fs -mkdir /WordCountTutorial/Input
```
# Movemos al directorio
```
hadoop fs -put <cargar ruta del archivo> <de la siguiente manera>
hadoop fs -put '/home/hadoop/Escritorio/WordCountTutorial/Input_data/Input_txt' /WordCountTutorial/Input
hadoop fs -ls /WordCountTutorial/Input
hadoop fs -ls /WordCountTutorial/Input/Input_txt
```
# Cambiamos el directorio al directorio tutorial:
```
cd /home/hadoop/Escritorio/WordCountTutorial
```

# compilamos el codigo java:
```
javac -classpath ${HADOOP_CLASSPATH}$ -d <incluir las direcciones del folder trabajado ejemplo tutorial_classes> <y la aplicación java>
```
# Este es un ejemplo del código anterior con los datos de la aplicación:
```
javac -classpath ${HADOOP_CLASSPATH}$ -d '/home/hadoop/Escritorio/WordCountTutorial/tutorial_classes' '/home/hadoop/Escritorio/WordCountTutorial/WordCount.java'
```
# Ponemos la salida en un jar file
```
jar -cvf firstTutorial.jar -C tutorial_classes/ .
```
# Ahora corremos el jar file en hadoop
```
hadoop jar '/home/hadoop/Escritorio/WordCountTutorial/firstTutorial.jar' WordCount /WordCountTutorial/Input /WordCountTutorial/Output
```
# Finalmente obtenemos las salidas
```
hadoop dfs -cat /WordCountTutorial/Output/*
```
# Otros comandos.
```
hadoop fs -ls /WordCountTutorial/Input
hadoop fs -cat /WordCountTutorial/Input/*

hadoop fs -rm -r /WordCountTutorial/Input
hadoop fs -mkdir /WordCountTutorial/Input

hadoop fs -put '/home/hadoop/Escritorio/WordCountTutorial/Input_data/Annex_1_Aplication.txt' /WordCountTutorial/Input

hadoop fs -rm -r /WordCountTutorial/Output

hadoop jar '/home/hadoop/Escritorio/WordCountTutorial/firstTutorial.jar' \
  WordCount \
  /WordCountTutorial/Input/Annex_1_Aplication.txt \
  /WordCountTutorial/Output

hadoop fs -cat /WordCountTutorial/Output/*

```

🖥️ Transferencia de Archivos entre VM (Linux) y PC (Windows) usando SCP

📌 Objetivo

Exportar archivos desde una máquina virtual Linux (VirtualBox) hacia el
PC anfitrión usando scp a través de red por IP.

⚙️ 1. Configuración de Red en VirtualBox

1.  Apagar la máquina virtual
2.  Ir a Configuración → Red
3.  Configurar:
    -   Adaptador 1: Activado
    -   Tipo: Adaptador puente (Bridge)
    -   Seleccionar la tarjeta de red real (WiFi o Ethernet)
    -   Modo promiscuo: Permitir todo

🌐 2. Obtener la IP de la Máquina Virtual

hostname -I ip a

Buscar una IP tipo: 192.168.x.x

🔄 3. Forzar asignación de IP

sudo dhclient -v enp0s3

📡 4. Verificar conectividad

ping 192.168.x.x

🔐 5. Instalar y activar SSH

sudo apt update sudo apt install openssh-server -y sudo systemctl start
ssh sudo systemctl enable ssh

📂 6. Transferir archivo con SCP

scp usuario@IP_VM:/ruta/archivo “C:”

Ejemplo: scp
hadoop@192.168.18.213:/home/hadoop/Escritorio/WordCountTutorial/WordCount.java
“C:G15”

⚠️ Problemas comunes

-   Connection timed out → SSH o red
-   Host inaccesible → Bridge mal configurado
-   No such file → ruta incorrecta

🧪 Prueba

touch prueba.txt scp hadoop@192.168.18.213:/home/hadoop/prueba.txt
“C:G15”

🎯 Conclusión

Requisitos: - Red Bridge - IP válida - SSH activo - Rutas correctas con
comillas
