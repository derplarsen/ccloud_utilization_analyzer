# Use Prometheus and Grafana against Confluent Cloud Metrics API

This repo is to help you learn what your utilization is like in Confluent Cloud, it is based on the work Damien Gasparina did more generally in the ccloudexporter project and conformed to make it much more straight forward to serve the purpose of directly getting into Grafana after starting to scrape metrics from Cloud Telemetry.

## Instructions:

Pre-requisite: Docker installed

1. Git clone this repo: 
```git clone https://github.com/derplarsen/ccloudexporter.git```
2. Enter the directory cloned
3. Set the environment variables for CCLOUD_USER, CCLOUD_PASSWORD, and CCLOUD_CLUSTER
```
export CCLOUD_USER=toto@confluent.io
export CCLOUD_PASSWORD=totopassword
export CCLOUD_CLUSTER=lkc-abc123
```
(You can get the Cluster ID from the **Cluster Settings** / **Cluster Details** area of the Confluent Cloud portal)

4. Run `docker-compose up -d` and it will start running everything
5. Visit http://localhost:3333 and you'll be presented with a *Grafana* login. The username is admin and the password is admin.
6. The first time you run it, you'll need to add Prometheus as a data source. Hover over the Gear icon and click Data Sources. Add a datasource for Prometheus with http://prometheus:9090 as the URL, then click ***Save & Test***.
7. Now that you have Prometheus added as the data source for Grafana, you can *import the dashboard*. For this, click on the **+** sign on the top left of the dashboard and click Import. Click "Upload .json file" which you will find in the **./grafana** folder. 

### Yay! You now have a prebuilt working Grafana dashboard sourcing from Prometheus which is collecting metrics from the Confluent Cloud Metrics API. 

~~ For additional reference below ~~

# ORIGINAL README from Damien: 

----------

# Prometheus exporter for Confluent Cloud Metrics API

A simple prometheus exporter that can be used to extract metrics from [Confluent Cloud Metric API](https://docs.confluent.io/current/cloud/metrics-api.html).
By default, the exporter will be exposing the metrics on [port 2112](http://localhost:2112)
To use the exporter, the following environment variables need to be specified:

* `CCLOUD_USER`: Your Confluent Cloud login, or the API Key created with `ccloud api-key create --resource cloud`
* `CCLOUD_PASSWORD`: Your Confluent Cloud password, or the API Key Secret when created with `ccloud api-key create --resource cloud`

`CCLOUD_USER` and `CCLOUD_PASSWORD` environment variables will be used to invoke the https://api.telemetry.confluent.cloud endpoint.

## Usage
```
./ccloudexporter -cluster <cluster_id>
````

### Options

```
Usage of ./ccloudexporter:
  -cluster string
    	Cluster ID to fetch metric for. If not specified, the environment variable CCLOUD_CLUSTER will be used
  -listener string
    	Listener for the HTTP interface (default ":2112")
  -delay int
    	Delay, in seconds, to fetch the metrics. By default set to 120, this, in order to avoid temporary data points. (default 120)
  -endpoint string
    	Base URL for the Metric API (default "https://api.telemetry.confluent.cloud/")
  -granularity string
    	Granularity for the metrics query, by default set to 1 minutes (default "PT1M")
  -no-timestamp
    	Do not propagate the timestamp from the the metrics API to prometheus
  -timeout int
    	Timeout, in second, to use for all REST call with the Metric API (default 60)
```

## Examples

### Building and executing
```
go get github.com/Dabz/ccloudexporter/cmd/ccloudexporter
go install github.com/Dabz/ccloudexporter/cmd/ccloudexporter
export CCLOUD_USER=toto@confluent.io
export CCLOUD_PASSWORD=totopassword
./ccloudexporter -cluster lkc-abc123
```

### Using docker
```
docker run \
  -e CCLOUD_USER=$CCLOUD_USER \
  -e CCLOUD_PASSWORD=$CCLOUD_PASSWORD
  -p 2112:2112
  dabz/ccloudexporter:latest -cluster lkc-abc123
```

### Using docker-compose
```
export CCLOUD_USER=toto@confluent.io
export CCLOUD_PASSWORD=totopassword
export CCLOUD_CLUSTER=lkc-abc123
docker-compose up -d
```

## How to build
```
go get github.com/Dabz/ccloudexporter/cmd/ccloudexporter
```

## Grafana
A Grafana dashboard is provided in [./grafana/](./grafana) folder.

![Grafana Screenshot](./grafana/grafana.png)
