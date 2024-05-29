> [!WARNING]
> En construcción


> [!IMPORTANT] Lectura recomendada
> - https://datawise.dev/9-issues-ive-encountered-when-setting-up-a-hadoop-spark-cluster-for-the-first-time


# Descarga e instalación en el server

## Descarga

Descargamos la última versión:
https://spark.apache.org/downloads.html

**User provided Apache Hadoop** ya que vamos a utilizar la versión de hadoop que ya tenemos instalada en /opt
![[03-sparkDescarga.png]]
## Instalación

```
Instalacion:

cd /opt/hadoop

wget https://www.apache.org/dyn/closer.lua/spark/spark-3.5.1/spark-3.5.1-bin-hadoop3.tgz

tar -zxvf spark-3.5.1-bin-hadoop3.tgz

ln -s spark-3.5.1-bin-hadoop spark

```

Descargar -> cambiar permisos, chmod.....

# Configuración

```
cd /opt/hadoop/spark/conf

cp spark-defaults.conf.template spark-defaults.conf

# Editamos
vi spark-defaults.conf

# Añadimos la línea
spark.master yarn

```

** EDITAR Archivo env **

Modificamos nuestro .bashrc, añadimos:
```
# Vamos a nuestra home
cd
# Editamos
vi .bashrc
# Añadimos las siguientes líneas al final
```
```
export SPARK_HOME=/opt/hadoop/spark
export PATH=$PATH:/opt/hadoop/spark/bin:/opt/hadoop/spark/sbin
export SPARK_DIST_CLASSPATH=$(hadoop classpath)

```

## Iniciamos spark

```
# Recargamos la nueva configuración del .bashrc
source .bashrc

# No hace falta, pero vamos al directorio donde estan los ejecutables
cd /opt/hadoop/spark/sbin

# Iniciamos el nodo master
./start-master.sh -h 0.0.0.0

```

Ya podemos acceder a la IP del servidor (o nombre), puerto 8080
En nuestro caso:
http://cloniciabd:8080/

![[03-sparkUI.png]]


# Descarga e instalación en los clientes


Iniciamos nodos:
```
./start-worker.sh spark://cloniciabd:7077
```

-----

.bashrc
`
```
export SPARK_HOME=/opt/hadoop/spark
export PATH=$PATH:/opt/hadoop/spark/bin:/opt/hadoop/spark/sbin
export SPARK_DIST_CLASSPATH=$(hadoop classpath)

```

source ~/.bashrc

-- Copiar a todos los nodos

-----
lanzar master
$ pwd
/opt/hadoop/spark/sbin
./start-master.sh

----
Lanzar workers
$ pwd
/opt/hadoop/spark/sbin

./start-worker.sh spark://cloniciabd:7077

---
Error en workers
org.apache.spark.deploy.worker.Worker
Causado por: java.lang.NoClassDefFoundError: org/slf4j/Logger

**source  /home/hadoop/.bashrc**

---
![[Pasted image 20240528135242.png]]

# Errores
## Error 1
Para probar si funciona vamos a ejecutar la consola de pyspark
```
cd /opt/hadoop/spark/bin

pyspark

```
Nos devuelve el siguiente error:
**/opt/hadoop/spark/python/pyspark/shell.py:74: UserWarning: Failed to initialize Spark session.**
```
Python 3.10.12 (main, Nov 20 2023, 15:14:05) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
/opt/hadoop/spark/python/pyspark/shell.py:74: UserWarning: Failed to initialize Spark session.
  warnings.warn("Failed to initialize Spark session.")
Traceback (most recent call last):
  File "/opt/hadoop/spark/python/pyspark/shell.py", line 69, in <module>
    spark = SparkSession._create_shell_session()
  File "/opt/hadoop/spark/python/pyspark/sql/session.py", line 1145, in _create_shell_session
    return SparkSession._getActiveSessionOrCreate()
  File "/opt/hadoop/spark/python/pyspark/sql/session.py", line 1161, in _getActiveSessionOrCreate
    spark = builder.getOrCreate()
  File "/opt/hadoop/spark/python/pyspark/sql/session.py", line 497, in getOrCreate
    sc = SparkContext.getOrCreate(sparkConf)
  File "/opt/hadoop/spark/python/pyspark/context.py", line 515, in getOrCreate
    SparkContext(conf=conf or SparkConf())
  File "/opt/hadoop/spark/python/pyspark/context.py", line 203, in __init__
    self._do_init(
  File "/opt/hadoop/spark/python/pyspark/context.py", line 296, in _do_init
    self._jsc = jsc or self._initialize_context(self._conf._jconf)
  File "/opt/hadoop/spark/python/pyspark/context.py", line 421, in _initialize_context
    return self._jvm.JavaSparkContext(jconf)
  File "/opt/hadoop/spark/python/lib/py4j-0.10.9.7-src.zip/py4j/java_gateway.py", line 1587, in __call__
    return_value = get_return_value(
  File "/opt/hadoop/spark/python/lib/py4j-0.10.9.7-src.zip/py4j/protocol.py", line 326, in get_return_value
    raise Py4JJavaError(
py4j.protocol.Py4JJavaError: An error occurred while calling None.org.apache.spark.api.java.JavaSparkContext.
: java.lang.NoClassDefFoundError: org/apache/hadoop/mapred/JobConf
	at org.apache.spark.api.java.JavaSparkContext.<init>(JavaSparkContext.scala:58)
	at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at java.base/jdk.internal.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.base/java.lang.reflect.Constructor.newInstance(Constructor.java:490)
	at py4j.reflection.MethodInvoker.invoke(MethodInvoker.java:247)
	at py4j.reflection.ReflectionEngine.invoke(ReflectionEngine.java:374)
	at py4j.Gateway.invoke(Gateway.java:238)
	at py4j.commands.ConstructorCommand.invokeConstructor(ConstructorCommand.java:80)
	at py4j.commands.ConstructorCommand.execute(ConstructorCommand.java:69)
	at py4j.ClientServerConnection.waitForCommands(ClientServerConnection.java:182)
	at py4j.ClientServerConnection.run(ClientServerConnection.java:106)
	at java.base/java.lang.Thread.run(Thread.java:829)
Caused by: java.lang.ClassNotFoundException: org.apache.hadoop.mapred.JobConf
	at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:581)
	at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:178)
	at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:527)
	... 13 more
```

# Running the Examples and Shell

Spark comes with several sample programs. Python, Scala, Java, and R examples are in the examples/src/main directory.

To run Spark interactively in a Python interpreter, use bin/pyspark:
```
./bin/pyspark --master "local[2]"
```

Sample applications are provided in Python. For example:
```
./bin/spark-submit examples/src/main/python/pi.py 10
```

To run one of the Scala or Java sample programs, use bin/run-example class params in the top-level Spark directory. (Behind the scenes, this invokes the more general spark-submit script for launching applications). For example:
```
./bin/run-example SparkPi 10
```

You can also run Spark interactively through a modified version of the Scala shell. This is a great way to learn the framework.
```
./bin/spark-shell --master "local[2]"
```
The --master option specifies the master URL for a distributed cluster, or local to run locally with one thread, or local[N] to run locally with N threads. You should start by using local for testing. For a full list of options, run the Spark shell with the --help option.

Since version 1.4, Spark has provided an R API (only the DataFrame APIs are included). To run Spark interactively in an R interpreter, use bin/sparkR:
```
./bin/sparkR --master "local[2]"
```

Example applications are also provided in R. For example:
```
./bin/spark-submit examples/src/main/r/dataframe.R
```

# Running Spark Client Applications Anywhere with Spark Connect
Spark Connect is a new client-server architecture introduced in Spark 3.4 that decouples Spark client applications and allows remote connectivity to Spark clusters. The separation between client and server allows Spark and its open ecosystem to be leveraged from anywhere, embedded in any application. In Spark 3.4, Spark Connect provides DataFrame API coverage for PySpark and DataFrame/Dataset API support in Scala.

To learn more about Spark Connect and how to use it, see Spark Connect Overview.

```
/opt/hadoop/spark/sbin$ ./start-connect-server.sh --packages org.apache.spark:spark-connect_2.12:3.5.1
```

ERROR:
pyspark.errors.exceptions.connect.SparkConnectGrpcException: (org.apache.hadoop.hdfs.server.namenode.SafeModeException) Cannot create directory /user/hadoop/.data/example/_temporary/0. Name node is in safe mode.

Hadoop está en SAFEMODE
```
hdfs dfsadmin -safemode leave
```

# WebUI

Cluster, puerto 8080
![[03-sparkclusterUI.png]]
Jobs, puerto 4040
![[03-sparkJobs.png]]

# Ejemplos código

```
'''requirements.txt'''
googleapis-common-protos==1.56.1
grpcio==1.64.0
grpcio-status==1.64.0
grpcio-tools==1.64.0
numpy==1.26.4
pandas==2.2.2
protobuf==5.27.0
py4j==0.10.9.7
pyarrow==16.1.0
pyspark==3.5.1
python-dateutil==2.9.0.post0
pytz==2024.1
setuptools==70.0.0
six==1.16.0
tzdata==2024.1
```

```
'''test_remote.py'''
from pyspark.sql import SparkSession

spark = SparkSession.builder.remote("sc://IP_SERVER:15002").getOrCreate()
df = spark.range(100)
df.write.mode("overwrite").parquet(".data/example2")
spark.stop()
```

