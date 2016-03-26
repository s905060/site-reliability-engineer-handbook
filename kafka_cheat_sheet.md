# Kafka Cheat Sheet

*Display Topic Information*

```bash
$ kafka-topics.sh --describe --zookeeper localhost:2181 --topic beacon
Topic:beacon	PartitionCount:6	ReplicationFactor:1	Configs:
	Topic: beacon	Partition: 0	Leader: 1	Replicas: 1	Isr: 1
	Topic: beacon	Partition: 1	Leader: 1	Replicas: 1	Isr: 1
```

*Add Partitions to a Topic*

```bash
$ kafka-topics.sh --alter --zookeeper localhost:2181 --topic beacon --partitions 3
WARNING: If partitions are increased for a topic that has a key, the partition logic or ordering of the messages will be affected
Adding partitions succeeded!
```

*Delete Topic*

```bash
$ kafka-run-class.sh kafka.admin.DeleteTopicCommand --zookeeper localhost:2181 --topic test
```

```bash
kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic file_acquire_complete
kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic job_result
kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic trigger_match
kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic event_result
```

```bash
$ kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic job_result
Created topic "job_result".
```

```bash
$ kafka-topics.sh --list --zookeeper localhost:2181
event_result
file_acquire_complete
job_result
trigger_match
```

```bash
$ kafka-console-producer.sh --broker-list localhost:9092 --topic test
```

```bash
$ kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning
```

