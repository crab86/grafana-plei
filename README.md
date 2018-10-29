# grafana-plei
Grafana bundled with (P)rometheus, (L)ogstash,(E)lasticsearch & (I)nfluxDB using Docker to play around. Simply start the whole stack with Docker-Compose (docker-compose up).

The PLEI-stack is based on the following official Docker images:

* [Grafana](https://hub.docker.com/r/grafana/grafana/)
* [Prometheus](https://hub.docker.com/r/prom/prometheus/)
* [Logstash](https://www.docker.elastic.co)
* [Elasticsearch](https://www.docker.elastic.co)
* [InfluxDB](https://hub.docker.com/_/influxdb/)
* [cAdvisor](https://hub.docker.com/r/google/cadvisor/)

**Note**:
cAdvisor is not started per Default (commented out in docker-compose.yml). So if you would like to have additional metrics to play around with simply uncomment the cAdvisor statement in docker-compose.yml. cAdvisor then pushes its metrics to InfluxDB (pre-provisioned datasource) and prometheus scrapes them aswell. You would then need to add additional Dashboards on your own to build some cAdvisor Graphs.

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

### Elasticsearch as datasource:
- Elasticsearch - Container Logs
- Elasticsearch Monitoring based on X-Pack stats
- Logstash Monitoring based on X-Pack stats - repeat by node
- Logstash Monitoring based on X-Pack stats - grouped by node

### InfluxDB as datasource:
- InfluxDB - InfluxDB internal metrics (based on 421)

### Prometheus as datasource:
- Prometheus  - Prometheus Stats (imported from DataSource)
- Prometheus - Prometheus 2.0 Stats (imported from DataSource)
- Prometheus - Grafana Internals (based on 3590)


## Requirements

### Host setup

1. Install [Docker](https://www.docker.com/community-edition#/download)
2. Install [Docker Compose](https://docs.docker.com/compose/install/)
3. Clone this repository
