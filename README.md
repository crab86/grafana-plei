# grafana-plei
Grafana bundled with (P)rometheus, (L)ogstash,(E)lasticsearch & (I)nfluxDB using Docker to play around. Simply start the whole stack with Docker-Compose (docker-compose up). No additinal agents are needed, all the metrics and logs in all dashboards are based up on internal statistics of the used databases and tools itself.

The PLEI-stack is based on the following official Docker images:

* [Grafana](https://hub.docker.com/r/grafana/grafana/)
* [Prometheus](https://hub.docker.com/r/prom/prometheus/)
* [Logstash](https://www.docker.elastic.co)
* [Elasticsearch](https://www.docker.elastic.co)
* [InfluxDB](https://hub.docker.com/_/influxdb/)
* [cAdvisor](https://hub.docker.com/r/google/cadvisor/)

**Note**:
cAdvisor is not started per Default (commented out in docker-compose.yml). So if you would like to have additional metrics to play around with simply uncomment the cAdvisor statement in docker-compose.yml. cAdvisor then pushes its metrics to InfluxDB (Grafana datasource already pre-provisioned) and prometheus scrapes them aswell. You would then need to add additional Dashboards on your own to build some cAdvisor Graphs.

## Pre-provisioned Datasources in Grafana
Grafana comes pre-provisioned with the following datasources:

- Elasticsearch
- Elasticsearch-monitoring
- Elasticsearch-logstash-monitoring
- InfluxDB_internal
- InfluxDB_cadvisor
- Prometheus

## Pre-provisioned Dashboards in Grafana
Moreover the following Dashboards are also pre-provisioned to Grafana when starting the PLEI-stack:

### Dashboards with Elasticsearch as datasource:
- Elasticsearch - Container Logs (**Note**: All containers send their logs to Logstash via syslog driver. The logs can be viewed in this dashboard)
- [Elasticsearch Monitoring based on X-Pack stats](https://grafana.com/dashboards/8642)
- [Logstash Monitoring based on X-Pack stats - repeat by node](
  https://grafana.com/dashboards/8470)
- [Logstash Monitoring based on X-Pack stats - grouped by node](https://grafana.com/dashboards/8467)

### Dashboards with InfluxDB as datasource:
- [InfluxDB - InfluxDB internal metrics (based on 421)](https://grafana.com/dashboards/421)

### Dashboards with Prometheus as datasource:
- [Prometheus  - Prometheus Stats (imported from Datasource)](https://github.com/grafana/grafana/tree/master/public/app/plugins/datasource/prometheus/dashboards/prometheus_stats.json)
- [Prometheus - Prometheus 2.0 Stats (imported from Datasource)](https://github.com/grafana/grafana/tree/master/public/app/plugins/datasource/prometheus/dashboards/prometheus_2_stats.json)
- [Prometheus - Grafana Metrics (imported from Datasource)](https://github.com/grafana/grafana/blob/master/public/app/plugins/datasource/prometheus/dashboards/grafana_stats.json)

## Viewable Metrics and Logs

### Elasticsearch & Logstash
Elasticsearch offers X-Pack statistics to monitor Elasticsearch and Logstash out-of-the-box. You simple need to enable it via elasticsearch.yml / logstash.yml config files. The statistics are written to special elasticsearch indices and datasources+dashboards for those indices are already pre-provisioned when starting the stack.
For additional Information please refer to the official documentation:

[Elasticsearch Monitoring ](https://www.elastic.co/guide/en/elasticsearch/reference/current/configuring-monitoring.html)

[Logstash Monitoring](https://www.elastic.co/guide/en/logstash/current/configuring-logstash.html)

In addition all used containers publish their logs to logstash's syslog input. All the logs can be viewed in a pre-provisioned dashboard aswell.

### InfluxDB
InflxuDB offers internal statistics as described in the official documentation:

[InfluxDB Internal monitoring](https://docs.influxdata.com/influxdb/v1.6/administration/server_monitoring/#internal-monitoring)

A datasource+dashboard using this internal database is already pre-provisioned when starting the PLEI stack.

### Prometheus
Prometheus and Grafana expose /metrics endpoints which are scraped using jobs "prometheus" and "grafana". Moreover cAdvisor also exposes metrics which are scraped with a job "cAdvisor" although cAdvisor container is disabled by default, see [Note](#grafana-plei).

The prometheus datasource is already pre-provisioned when starting the PLEI stack. The datasource also comes with dashboards, those are pre-provisioned as well.

## Requirements

### Host setup

1. Install [Docker](https://www.docker.com/community-edition#/download)
2. Install [Docker Compose](https://docs.docker.com/compose/install/)
3. Clone this repository

## Usage

### Bringing up the stack

Start the PLEI stack using `docker-compose`:

```bash
$ docker-compose up
```
Grafana can then be accessed via [http://localhost:3000](http://localhost:3000) with a web browser (default login is admin:admin). Elasticsearch typically needs some time to completely start (around 2min.) so the dashboards with Elasticsearch as datasource may not be ready immediately.

By default, the PLEI stack exposes the following ports:
* 3000: Grafana
* 5140: Logstash Syslog UDP input
* 8086: InfluxDB HTTP
* 9090: Prometheus server
* 9200: Elasticsearch HTTP
* 9600: Logstash monitoring  
