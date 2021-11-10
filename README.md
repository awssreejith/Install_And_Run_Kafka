# Install And Run Apache Kafka
This repo contains notes about how to install, configure, run and test Apache kafka

Follow the below link for reference

https://www.digitalocean.com/community/tutorials/how-to-install-apache-kafka-on-ubuntu-18-04


Make a new kafka user first
---------------------------

sudo useradd kafka -m

sudo passwd kafka

sudo adduser kafka sudo

su -l kafka

Note: password i used is "kafka" for user kafka

Note: By default the shell will be csh. chnage it to bash by chnaging in /etc/passwd file 


Create Download directory uder /home/kafka
------------------------------------------

sudo curl https://downloads.apache.org/kafka/2.6.2/kafka_2.12-2.6.2.tgz  -o ~/Downloads/kafka.tgz

mkdir ~/kafka && cd ~/kafka

cd ~/kafka

tar -xvzf ~/Downloads/kafka.tgz --strip 1

After the above do the following

vi ~/kafka/config/server.properties

add below line

delete.topic.enable = true

Note: I am not making kafka and zookeper to start as service during system start up. Please follow the above link to do so

https://www.digitalocean.com/community/tutorials/how-to-install-apache-kafka-on-ubuntu-18-04

Start Zookeper first
--------------------
Open two terminal and issue the following

cd /home/ubuntu/kafka/bin

sudo ./zookeeper-server-start.sh ../config/zookeeper.properties 

sudo ./kafka-server-start.sh ../config/server.properties 

Once above is done you can check the health of Kafka as below

sudo journalctl -u kafka

To Test the Kafka
-----------------
To test we can create a producer and consumer and post a message to a topic [for example- Sreejith_Topic_1]

Step-1:- Create a topic named Sreejith_Topic_1

kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic Sreejith_Topic_1

Step-2:- Publish the string "Hello, World" to the TutorialTopic topic by typing

echo "Hello, World" | ./kafka-console-producer.sh --broker-list localhost:9092 --topic Sreejith_Topic_1 > /dev/null

Step-3:- Consume the above message from a consumer as below

./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic Sreejith_Topic_1 --from-beginning



NOTE: If there are any errors related to starting or stopping simply remove the folder sudo rm -rf /tmp/kafka-logs/



