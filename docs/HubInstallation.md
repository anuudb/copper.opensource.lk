# Copper hub Installation Process



##Copper-hub
<p align="justify">
This repository contains source code for copper-hub which is the alerting, monitoring and update handling system for Copper.

First, create grafana docker image using ./grafana-image/Dockerfile. (Read the ./grafana-image/README.md before building image)
</p>

```
docker build -t graf .
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
    - Type: Prometheus
    - Url: http://prometheus.monitoring.svc.cluster.local:9090/
    - Add 

####Import the grafana dashboard from "./prometheus-master/grafana_dashboards/dashboard_1.json" to grafana.

* Brows [Dashboards / Manage / import}
    - Name: Kubernetes Pod Resources
    - Location: /prometheus-master/grafana_dashboards/dashboard_1.json
    - import

####Create a "Notification channel" to make sure that alert mails will receive for the right address.

* Brows [Alerting / Notification channels / New channel]
    - Name: Email
    - Type: Email
    - Email addresses: Your email address
    - import