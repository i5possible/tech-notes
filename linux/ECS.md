# ECS配置

## SSH PORT
```
vim /etc/ssh/sshd_config
    add
	   PORT 15312
service sshd restart
```

## Hostname
vim /etc/hostname

## Hosts
vim /etc/hosts
```
10.31.91.90 kafka1
10.31.91.45 kafka2
10.31.91.90 zookeeper1
10.31.91.45 zookeeper2
10.29.167.198 zookeeper3
```

## Java
JDK:  
yum install java-1.8.0-openjdk

JPS:  
yum install java-1.8.0-openjdk-devel.x86_64

## Zookeeper

### zookeeper.properties (or zoo.cfg)
```
dataDir=/tmp/zookeeper
clientPort=2181
maxClientCnxns=0

tickTime=2000
initLimit=10
syncLimit=5

server.1=zookeeper1:2888:3888
server.2=zookeeper2:2888:3888
server.3=zookeeper3:2888:3888
```

### Myid
mkdir /tmp/zookeeper

vim /etm/zookeeper/myid (myid is the id of the zookeeper)

### Connect
```
zookeeper-shell.sh -server zookeeper1:2181
or
zkCli.sh -server zookeeper1:2181
```
## Kafka
```
brokerId=0
zookeeper.connect=zookeeper1:2181:zookeeper2:2181:zookeepeer3:2181
```

```
jps: verify the kafka
pid Kafka
log:
```

## Transformation
```
scp -r -P 15312 /opt/zookeeper-3.3.6 root@10.31.91.45:/opt
scp -r -P 15312 /opt/kafka_2.11-1.0.0 root@10.31.91.45:/opt
```
