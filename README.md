# mstr_system_monitor

# install software & packages
sudo yum install gcc
sudo yum install python3-devel OR sudo yum install python3-dev
sudo pip3 install psutil
pip3 install kafka-python
pip3 install kafka

# start zookeeper & kafka server in the background
cd /opt/kafka_2.12-2.3.0
bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
bin/kafka-server-start.sh -daemon config/server.properties

# create/list/describe topic
bin/kafka-topics.sh --create --zookeeper 192.168.56.101:2181 --replication-factor 1 --partitions 1 --topic system_monitor
bin/kafka-topics.sh --list --zookeeper 192.168.56.101:2181
bin/kafka-topics.sh --describe --zookeeper 192.168.56.101:2181 --topic system_monitor

# run python consumer & producer program 
python3 producer.py
python3 consumer.py

# cleanup
cd /opt/kafka_2.12-2.3.0
bin/kafka-topics.sh --delete --zookeeper 192.168.56.101:2181 --topic system_monitor
bin/zookeeper-server-stop.sh -daemon config/zookeeper.properties
bin/kafka-server-stop.sh -daemon config/server.properties