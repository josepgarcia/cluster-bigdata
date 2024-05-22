> [!NOTE] En construcción


> [!warning] IMPORTANT
> https://datawise.dev/9-issues-ive-encountered-when-setting-up-a-hadoop-spark-cluster-for-the-first-time


Puerto spark 8080

http://cloniciabd:8080/

![[Pasted image 20240520201921.png]]



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
