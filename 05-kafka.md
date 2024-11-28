# Kafka

## Print queue size for all partitions of a topic

`bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group <CONSUMER_GROUP> --describe | grep "<TOPIC> " | awk '{total += $6; print total}' | tail -1`

## Change Kafka Connect log level

- Get current level: `curl http://localhost:8083/admin/loggers/org.apache.kafka.connect.runtime.WorkerSourceTask | jq`
- Set level: `curl -s -X PUT -H "Content-Type:application/json" http://localhost:8083/admin/loggers/org.apache.kafka.connect.runtime.WorkerSourceTask -d '{"level": "INFO"}'`

## Increase max message size on a topic

`./<KAFKA_HOME>/bin/kafka-configs.sh --bootstrap-server localhost:9092 --alter --entity-type topics --entity-name <TOPIC_NAME> --add-config max.message.bytes=<BYTES_SIZE>`

## Resources

- https://medium.com/@TimvanBaarsen/apache-kafka-cli-commands-cheat-sheet-a6f06eac01b#a1a2
- https://docs.confluent.io/platform/current/connect/logging.html