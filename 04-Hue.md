> [!WARNING]
> En construcción

https://github.com/cloudera/hue/blob/master/tools/docker/hue/README.md


https://github.com/cloudera/hue

Como root:
```
cd opt

git clone https://github.com/cloudera/hue.git

cd hue

```

# Docker

```
docker run -it -p 8888:8888 gethue/hue:latest
```

cloniciabd:8888

admin  /  pass_de_sempre

```
docker run -d -p 8888:8888 --name ghue gethue/hue:latest

```

# Otra

https://kyuubi.readthedocs.io/en/v1.5.2-incubating/quick_start/quick_start_with_hue.html

Copy a configuration template from Hue Docker image.
```
docker run --rm gethue/hue:latest cat /usr/share/hue/desktop/conf/hue.ini > hue.ini
```

Modificaimos hue.ini
```
time_zone=Europe/Madrid


[[hdfs_clusters]]
  # HA support by using HttpFs

[[[default]]]
# Enter the filesystem uri
#fs_defaultfs=hdfs://localhost:8020
fs_defaultfs=hdfs://172.28.255.253:8020

## webhdfs_url=http://localhost:50070/webhdfs/v1
webhdfs_url=http://172.28.255.253:50070/webhdfs/v1
hadoop_conf_dir=$HADOOP_CONF_DIR when set or '/opt/hadoop/conf'

```

Spark
https://docs.gethue.com/administrator/configuration/connectors/#apache-spark
```
[spark]
# The Livy Server URL.
livy_server_url=http://172.28.255.253:8998

--------

## Show the notebook menu or not
show_notebooks=true
```

Modificar /opt/hadoop/etc/hadoop/core-site.xml
```
<property>
  <name>hadoop.proxyuser.hue.hosts</name>
  <value>*</value>
</property>
<property>
  <name>hadoop.proxyuser.hue.groups</name>
  <value>*</value>
</property>
```

Iniciamos
```
docker run -p 8888:8888 -v $PWD/hue.ini:/usr/share/hue/desktop/conf/hue.ini gethue/hue:latest
```
