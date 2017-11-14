## Start
``` 
/opt/kafka_2.11-1.0.0/bin/kafka-server-stop.sh
/opt/kafka_2.11-1.0.0/bin/zookeeper-server-stop.sh 

/opt/kafka_2.11-1.0.0/bin/zookeeper-server-start.sh -daemon /opt/kafka_2.11-1.0.0/config/zookeeper.properties
/opt/kafka_2.11-1.0.0/bin/kafka-server-start.sh -daemon /opt/kafka_2.11-1.0.0/config/server.properties

/opt/kafka_2.11-1.0.0/bin/zookeeper-shell.sh zookeeper1:2181
```