## Start
``` 
/opt/kafka_2.11-0.9.0.1/bin/kafka-server-stop.sh
/opt/kafka_2.11-0.9.0.1/bin/zookeeper-server-stop.sh 

/opt/kafka_2.11-0.9.0.1/bin/zookeeper-server-start.sh -daemon /opt/kafka_2.11-0.9.0.1/config/zookeeper.properties
/opt/kafka_2.11-0.9.0.1/bin/kafka-server-start.sh -daemon /opt/kafka_2.11-0.9.0.1/config/server.properties

/opt/kafka_2.11-0.9.0.1/log

/opt/kafka_2.11-0.9.0.1/bin/zookeeper-shell.sh zookeeper1:2181

ls /brokers/ids
```