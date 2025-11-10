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
