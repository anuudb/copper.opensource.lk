# Copper hub Installation Process



##Copper-hub
<p align="justify">
This repository contains source code for [copper-hub](https://github.com/lsflk/copper-hub) which is the alerting, monitoring and update handling system for Copper. You may access it using http://localhost:3000 after completing belove steps.

First, create grafana docker image using ./grafana-image/Dockerfile. (Read the ./grafana-image/README.md before building image)
</p>

```
docker build -t graf ./grafana-image/.
```
###Quick start
To quickly start all the things just do this:

    kubectl apply --filename ./prometheus-master/manifests-all.yaml

    kubectl apply --filename ./prometheus-master/grafana.yaml

<p align="justify">
This will create the namespaces monitoring and grafana and will bring up all components there.

Use port 3000 to access grafana.

To shut down all components again you can just delete that namespace:
</p>

    kubectl delete namespace monitoring

    kubectl delete namespace grafana

After installing, it is must to create a datasource in grafana as "prometheus" and local URL would be "http://prometheus.monitoring.svc.cluster.local:9090/".

####Configure Prometheus data source for Grafana.

* Brows [Grafana UI / Data Sources / Add data source]
    - Name: prometheus
    - Url: http://prometheus.monitoring.svc.cluster.local:9090/
    - Add 

####Configure Elastic data source for Grafana.

* Brows [Grafana UI / Data Sources / Add data source]
    - Name: Elasticsearch
    - Url: http://elasticsearch.copperhub.svc.cluster.local:9200/
    - Time field name: @timestamp
    - Add 

####Import grafana dashboard for prometheus metrices details from "./grafana_dashboards/k8s_dashboard.json" to grafana.

* Brows [Dashboards / Manage / import}
    - Name: Kubernetes Pod Resources
    - Location: /grafana_dashboards/k8s_dashboard.json
    - import

####Import grafana dashboard for elastic log analysis from "./grafana_dashboards/elasticSearch_dashboard.json" to grafana.

* Brows [Dashboards / Manage / import}
    - Name: Kubernetes Pod Resources
    - Location: /grafana_dashboards/elasticSearch_dashboard.json
    - import

####Create a "Notification channel" to make sure that alert mails will receive for the right address.

* Brows [Alerting / Notification channels / New channel]
    - Name: Email
    - Type: Email
    - Email addresses: Your email address
    - import