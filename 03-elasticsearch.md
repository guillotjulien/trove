# Elasticsearch

## Slowlog

Slowlog is enabled per index (conf of log level and location are in ES `log4j2.properties` file):
```
curl -X PUT "localhost:9200/<INDEX_NAME>/_settings?pretty" -H 'Content-Type: application/json' -d'
{
  "index.search.slowlog.threshold.query.warn": "10s",
  "index.search.slowlog.threshold.query.info": "5s",
  "index.search.slowlog.threshold.query.debug": "2s",
  "index.search.slowlog.threshold.query.trace": "500ms",
  "index.search.slowlog.threshold.fetch.warn": "1s",
  "index.search.slowlog.threshold.fetch.info": "800ms",
  "index.search.slowlog.threshold.fetch.debug": "500ms",
  "index.search.slowlog.threshold.fetch.trace": "200ms"
}
'
```

Parsing can be done using this script, then importing the data in Mongo (or Duckdb) for further analysis
```
#!/bin/bash

# Check if the input file is provided as an argument
if [ $# -ne 1 ]; then
  echo "Usage: $0 <input_file>"
  exit 1
fi

# Input file containing log lines
input_file="$1"

while IFS= read -r line; do
  echo "$line" | awk -F'[][ ,]+' '{
  for (i=1; i<=NF; i++) {
    if ($i ~ /index/) {
      idx = $(i+2)
    } else if ($i ~ /took_millis/) {
      took_millis = $(i+1)
    } else if ($i ~ /total_hits/) {
      total_hits = $(i+1)
    } else if ($i ~ /source/) {
      source_start = index($0, "source[") + 7
      source_end = index($0, "], id[]") - 1
      source = substr($0, source_start, source_end - source_start + 1)
    }
  }
  printf("{\"index\":\"%s\",\"took_millis\":%s,\"total_hits\":%s,\"query\":%s}\n", idx, took_millis, total_hits, source)
}'
done < "$input_file"
```

## Resources

- https://weng.gitbooks.io/elasticsearch-in-action/content/chapter11_administering_your_cluster/113monitoring_for_bottlenecks.html
- https://www.elastic.co/guide/en/elasticsearch//reference/master/fix-common-cluster-issues.html
