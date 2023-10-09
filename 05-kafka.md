# Kafka

## Print queue size for all partitions of a topic

`bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group <CONSUMER_GROUP> --describe | grep "<TOPIC> " | awk '{total += $6; print total}' | tail -1`

## Resources

- https://medium.com/@TimvanBaarsen/apache-kafka-cli-commands-cheat-sheet-a6f06eac01b#a1a2
