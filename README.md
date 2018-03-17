# Kafka-bashfile
sudo chmod 600 ~/.ssh/ama6.pem 
ssh -i ama6.com ubontu@52.87.189.221
// I connected to ubontu
// Update Java
sudo apt-get update
// Install Java
sudo apt-get install default-jre
// add kafka user
sudo useradd kafka -m
// creat a password
sudo passwd kafka
//add user to group soudo
sudo adduser kafka sudo
// install zookeeper. it helps us to have enough time to process data
sudo apt-get install zookeeperd
// I test the connection : ruok   ---> Imok
telnet localhost 2181
// I found it interesting because there is no need to set pasword and user name on soudo.As Kafka is a network application creating a non sudo user specifically for Kafka minimizes the risk if the machine is to be compromised.
sudo adduser --system --no-creat-home --disabled-password --disabled-login kafka
cd ~
//make directory
mkdir -p ~/Downloads
//get file 
wget "http://mirror.bit.edu.cn/apache/kafka/1.0.0/kafka_2.11-1.0.0.tgz" -O ~/Downloads/kafka.tgz
mkdir -p ~/kafka && cd ~/kafka
// check the integrity of downloaded file
kafka.apache.org/KEYS | gpg --import
http://kafka.apache.org/KEYS | gpg --import
cd Downloads
cd ../kafka
//uncompress the file
tar -xvzf ~/Downloads/kafka.tgz --strip 1
// I added "delete.topic.enable=true" to the end of the file so I would be able to delete topics.
vi /kafka/config/server.properties
// adding memory
export KAFKA_HEAP_OPTS="-Xmx256M -Xms256M"
// run the server
~/kafka/bin/kafka-server-start.sh ~/kafka/config/server.properties
nohup ~/kafka/bin/kafka-server-start.sh ~/kafka/config/server.properties > ~/kafka/kafka.log 2>&1 &
// run zookeeper
~/kafka/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
~/kafka/bin/kafka-topics.sh --list --zookeeper localhost:2181
// start producer
~/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
//start consumer
~/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
~
