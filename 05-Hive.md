> [!WARNING]
> En construcción


# Descarga

https://hive.apache.org/general/downloads/

```
cd /opt/hadoop

wget https://dlcdn.apache.org/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz

tar -zxvf apache-hive-3.1.3-bin.tar.gz

ln -s apache-hive-3.1.3-bin hive

```

# Configuración
```
.bashrc


## hive
export HIVE_HOME=/opt/hadoop/hive
export PATH=$PATH:$HIVE_HOME/bin



```

Dentro de la carpeta conf de hive:
```
# Copiamos los siguientes templates por defecto 
cp hive-default.xml.template hive-site.xml cp hive-env.sh.template hive-env.sh cp hive-exec-log4j2.properties.template hive-exec-log4j2.properties cp hive-log4j2.properties.template hive-log4j2.properties cp beeline-log4j2.properties.template beeline-log4j2.properties

```

hive-env.sh
```
export HADOOP_HOME=/opt/hadoop 
export HIVE_CONF_DIR=/opt/hadoop/hive/conf
```

Creamos unas carpetas en hdfs para trabajar con hive
```
hdfs dfs -mkdir /tmp
hdfs dfs -chmod g+w /tmp
hdfs dfs -mkdir -p /user/hive/warehouse
hdfs dfs -chmod g+w /user/hive/warehouse
```

