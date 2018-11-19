# ELK Stack

A dockerized ELK stack for easy access to logs.

Docker Logs ->  Logstash    ->       ElasticSearch    ->      Kibana 
               (aggregate,          (Index & Store)       (Visualise Logs)
               transform & 
               enrich logs)      


## ElasticSearch

Ports:  9200 (Elasticsearch HTTP), 9300 (Elasticsearch TCP transport)

Test: `curl localhost:9200`

## Logstash

Ports: 5000 (Logstash TCP input), 9600

Test: `telnet localhost:5000`

## Kibana

Ports: 5601

Dashboard: Open browser at `localhost:5601`

## Create Index pattern in Kibana

### Inject some log entries into Logstash

```
$ nc localhost 5000 < /path/to/logfile.log
```

or by running a conatiner that sends its log to logstash:

```
$ docker run --log-driver=syslog --log-opt syslog-address=tcp://localhost:5000 hello-world
```

### Use Kibana UI

You need to inject data into Logstash before being able to configure a Logstash index pattern via the Kibana web UI. Then all you have to do is hit the *Create* button.

### Use the Kibana API

Create an index pattern via the Kibana API:

```
$ curl -XPOST -D- 'http://localhost:5601/api/saved_objects/index-pattern' \
    -H 'Content-Type: application/json' \
    -H 'kbn-version: 6.4.2' \
    -d '{"attributes":{"title":"logstash-*","timeFieldName":"@timestamp"}}'
```

The created pattern will automatically be marked as the default index pattern as soon as the Kibana UI is opened for the first time.