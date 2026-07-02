# Tempo
Below Steps will help to install Tempo for Tracing  a sample application 


# 1. Install Loki-Stack  

Install Loki and promtail combined in helm chart which will capture the logs from Nodes and send it to Loki Datasource which will be added as a datasource in Grafana.  

    helm upgrade --install loki grafana/loki-stack


# 2. Install Tempo  

Grafana Tempo is an open source, easy-to-use and high-scale distributed tracing backend. Tempo is cost-efficient, requiring only object storage to operate, and is deeply integrated with Grafana, Prometheus, and Loki  

    helm repo add grafana https://grafana.github.io/helm-charts
    helm upgrade --install  tempo grafana/tempo

# 3. Install Kube-Prometheus-Stack  
Reference Documentation is as below:  

https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack

This will installs the kube-prometheus stack, a collection of Kubernetes manifests, Grafana dashboards, and Prometheus rules combined with documentation and scripts to provide easy to operate end-to-end Kubernetes cluster monitoring with Prometheus using the Prometheus Operator.

Below Steps needs to be followed: 

    helm upgrade --install grafana prometheus-community/kube-prometheus-stack -n observability --values tempo-config.yaml

# 4. Install Sample Application for ingesting Traces to Grafana  

Below Steps needs to be followed: 

    kubectl apply -f tempo-test-dep.yaml
    kubectl apply -f tempo-test-svc.yaml

# 5. Validate the metrics on Grafana
To Search all of the time series data points grouping by job  in query  

    count({__name__=~".+"}) by (job)

# 6. Import Node Exporter Full Dashboard  

Reference Documentation:
https://grafana.com/grafana/dashboards/1860-node-exporter-full/ 