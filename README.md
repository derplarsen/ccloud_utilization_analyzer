# Use Prometheus and Grafana against Confluent Cloud Metrics API

This repo is to help you learn what your utilization is like in Confluent Cloud, it is based on the work Damien Gasparina did more generally in the ccloudexporter project and conformed to make it much more straight forward to serve the purpose of getting directly into a pre-built Grafana dashboard and pre-loaded Prometheus datasource, after starting to scrape metrics from Cloud Telemetry.

## Instructions:

Pre-requisite: Docker installed

1. Git clone this repo: 
```git clone https://github.com/derplarsen/ccloud_utilization_analyzer.git```
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

 **Yay! You now have a prebuilt working Grafana dashboard sourcing from Prometheus which is collecting metrics from the Confluent Cloud Metrics API. **

### **Q**: I don't have any data showing? 

### **A**: Prometheus is a database that needs to be populated, it will start scraping the Metrics API

### **Q**: When can I expect to see my metrics rolling in? 

### **A**: Metrics API data is populated every couple of minutes so you shouldn't have to wait long. Reload your Grafana dashboard page if necessary.

### **Q**: How do I access Prometheus directly?

### **A**: Provided you don't have any port conflicts, you should be able to get to Prometheus to run expressions on http://localhost:9090

### **Q**: What if I want to leverage the exporter directly using something other than Prometheus?

### **A**: Great! You're savvy with telemetry exporters, as specified in Damien's README, you can hit it at localhost:2112
