# Mongo

## Monitor index build

- `db.<COLLECTION>.stats().indexBuilds`
- `db.currentOp(true).inprog.forEach(function(op){ if(op.msg!==undefined) print(op.msg) })` (above should cut it for all now)

## Long running query

`db.currentOp({ "active" : true, "secs_running" : { "$gt" : 100 } })` where 100 is in seconds (need write access)

## Collection Fragmentation

Frag higher than 1 is bad, it means we are wasting storage. Fragmentation can happen when mass deleting stuff from collections or over time.

We can use `compact` now. After 4.4 operation isn't blocking anymore: https://www.mongodb.com/docs/manual/reference/command/compact/.
Previously the best way was to swap the collection with a new one and backfill the old one into the new one.

``` js
for (const collection of db.getCollectionNames()) {
    const s = db.getCollection(collection).stats()
    const frag = s.storageSize / (s.size + s.totalIndexSize)

    print(collection + ": " + (s['wiredTiger']['block-manager']['file bytes available for reuse'] / 1024 / 1024 / 1024) + " (frag: " + frag + ")") // frag higher than 1 is bad
}
```

## Get all keys of a collection

DO NOT RUN ON LARGE COLLECTIONS

``` js
db.<COLLECTION>.aggregate([
  {"$project":{"arrayofkeyvalue":{"$objectToArray":"$$ROOT"}}},
  {"$unwind":"$arrayofkeyvalue"},
  {"$group":{"_id":null,"allkeys":{"$addToSet":"$arrayofkeyvalue.k"}}}
])
```

## Knowing when a document was created in Mongo

`ObjectId` contains a timestamp.

```
ObjectId('5f16d7acead770dbef558866').getTimestamp()
```

## Query by creation date via `_id`

Useful if you don't have an index on a date field.

```
function objectIdWithTimestamp(timestamp) {
  /* Convert string date to Date object (otherwise assume timestamp is a date) */
  if (typeof(timestamp) == 'string') {
    timestamp = new Date(timestamp);
  }
  /* Convert date object to hex seconds since Unix epoch */
  var hexSeconds = Math.floor(timestamp/1000).toString(16);
  /* Create an ObjectId with that hex timestamp */
  var constructedObjectId = ObjectId(hexSeconds + "0000000000000000");
  return constructedObjectId
}

db.getCollection(<COLLECTION>).find({ _id: { $gt: objectIdWithTimestamp('2022-11-08') } });
```

## Number of connections per user

```
db.currentOp(true).inprog.reduce(
  (accumulator, connection) => {
    user = connection.effectiveUsers ? connection.effectiveUsers[0].user : "Internal";
    accumulator[user] = (accumulator[user] || 0) + 1;
    accumulator["TOTAL_CONNECTION_COUNT"]++;
    return accumulator;
  },
  { TOTAL_CONNECTION_COUNT: 0 }
)
```

## Number of connections per IP

```
db.currentOp(true).inprog.reduce((acc, d) => {
  if (d.client) {
    var ip = d.client.substring(0, d.client.length - 6)
    acc[ip] = (acc[ip] || 0) + 1;
  }

  return acc;
}, {})
```

## Configure Kafka Connect to publish collection change streams to Kafka

```
name=<UNIQUE_AND_DESCRIPTIVE_CONNECTOR_NAME>
tasks.max=1
connector.class=com.mongodb.kafka.connect.MongoSourceConnector
key.converter=org.apache.kafka.connect.storage.StringConverter
value.converter=org.apache.kafka.connect.storage.StringConverter
connection.uri=<MONGO_REPLICASET_URI>

database=<MONGO_DATABASE>
collection=<MONGO_COLLECTION>

topic.prefix=cleanmgo
change.stream.full.document=updateLookup

# This extract the documentKey into a field so that it can be used to partition the topic
transforms=ExtractDocumentKey,ExtractDocumentIdKey

transforms.ExtractDocumentKey.field=documentKey
transforms.ExtractDocumentKey.type=org.apache.kafka.connect.transforms.ExtractField$Key

# Don't apply transform to heartbeats
transforms.ExtractDocumentKey.predicate=IsHeatbeat
transforms.ExtractDocumentKey.negate=true

transforms.ExtractDocumentIdKey.field=_id
transforms.ExtractDocumentIdKey.type=org.apache.kafka.connect.transforms.ExtractField$Key

# Don't apply transform to heartbeats
transforms.ExtractDocumentIdKey.predicate=IsHeatbeat
transforms.ExtractDocumentIdKey.negate=true

predicates=IsHeatbeat
predicates.IsHeatbeat.type=org.apache.kafka.connect.transforms.predicates.TopicNameMatches
predicates.IsHeatbeat.pattern=__mongodb_heartbeats

output.format.value=json
output.json.formatter=com.mongodb.kafka.connect.source.json.formatter.SimplifiedJson
output.schema.key={ "name": "DocumentKey", "type": "record", "namespace": "mynamespace.mongodb.avro", "fields": [ { "name": "documentKey", "type": { "name": "documentKeyField", "type": "record", "fields": [ { "name": "_id", "type": "string" } ] } } ] }
output.format.key=schema

# rare updates - make sure we don't run into errors
heartbeat.interval.ms=3600000
errors.tolerance=all
errors.log.enable=true
errors.log.include.messages=true

# Optionally we can define a pipeline to filter out some documents
# pipeline=[{ "$match": { "fullDocument.whatever": true } }]

# We can also bootstrap the topic using the current content of the collection
# copy.existing=true
# copy.existing.pipeline=[{ "$match": { "whatever": true } }]
```

## Resources

- https://medium.com/idealo-tech-blog/advanced-mongodb-performance-tuning-2ddcd01a27d2
- https://github.com/idealo/mongodb-slow-operations-profiler (Go profiler is better)
- https://learn.mongodb.com/courses/ilt-mdb200-mongodb-optimization-and-performance
- https://learn.mongodb.com/courses/ilt-oa610-primary-session-use-to-create-new-courses
- https://docs.mongodb.com/manual/reference/method/db.currentOp/
- https://www.mongodb.com/blog/post/performance-best-practices-indexing
- https://www.mongodb.com/blog/post/performance-best-practices-benchmarking (All from the series)
- https://www.mongodb.com/blog/post/building-with-patterns-a-summary
- https://github.com/johnlpage/POCDriver
