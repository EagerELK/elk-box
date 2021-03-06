# Example ELK Box

This is the vagrant setup for a simple box running the ELK stack. It will make Elasticsearch available on port 9200 and Kibana on port 5601 of the host.

It setup's up Logstash as a standin for a syslog server, and collects both syslog events and events generated by the [`collectd`](https://www.elastic.co/guide/en/logstash/current/plugins-codecs-collectd.html) input.

## Howto

```bash
clone https://github.com/EagerElk/elk-box
cd elk-box
vagrant up
```

You can not browse to [localhost:5601](http://localhost:5601) to see your events flowing in.

## Extra

Get [the slides that accompany the talk](http://jrgns.net/talks/Moar_Logs.pdf). Not very detailed, but hey :)

Some queries that you can run against Elasticsearch

```bash
curl 'localhost:9200/_search/' -d '{"aggs":{"events_per":{"date_histogram":{"field":"@timestamp", "interval":"minute"}}}}'
curl 'localhost:9200/_search/?pretty' -d '{"aggs":{"events_per":{"date_histogram":{"field":"@timestamp", "interval":"minute"}}}}'
curl 'localhost:9200/logstash-*/_search/?pretty' -d '{"aggs":{"events_per":{"date_histogram":{"field":"@timestamp", "interval":"hour"}}}}'
curl 'localhost:9200/logstash-*/_search/?pretty' -d '{"size": 0, "aggs":{"events_per":{"date_histogram":{"field":"@timestamp", "interval":"hour"}}}}'
curl 'localhost:9200/logstash-*/_search/?pretty' -d '{"size": 0, "aggs":{"events_per":{"date_histogram":{"field":"@timestamp", "interval":"hour", "min_doc_count": 0\}}}}'


curl 'localhost:9200/_search/?q=type:syslog&pretty'
curl 'localhost:9200/_search/?q=type:syslog&pretty'

curl 'localhost:9200/_search/?q=type:syslog&pretty'
curl 'localhost:9200/_search/?pretty' -d '{"query":{"term":{"type":"syslog"}}}'

curl 'localhost:9200/_search/?pretty' -d '{"query":{"filtered":{"query":{"term":{"type":"syslog"}},"filter":{"range":{"@timestamp":{"gte":"now-1h"}}}}}}'
```
