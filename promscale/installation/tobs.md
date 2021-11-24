# Tobs - The Observability Stack for Kubernetes

Tobs is a tool that aims to make it as easy as possible to install a full observability
stack into a Kubernetes cluster. Currently this stack includes:

<img src="<UPLOAD_TOBS_IMAGE_TO_S3_AND_REFER_IT_HERE>" alt="Tobs Architecture Diagram" width="800"/>

 * [Kube-Prometheus](https://github.com/prometheus-operator/kube-prometheus#kube-prometheus) the Kubernetes monitoring stack
    * [Prometheus](https://github.com/prometheus/prometheus) to collect metrics
    * [AlertManager](https://github.com/prometheus/alertmanager#alertmanager-) to fire the alerts
    * [Grafana](https://github.com/grafana/grafana) to visualize what's going on
    * [Node-Exporter](https://github.com/prometheus/node_exporter) to export metrics from the nodes
    * [Kube-State-Metrics](https://github.com/kubernetes/kube-state-metrics) to get metrics from kubernetes api-server
    * [Prometheus-Operator](https://github.com/prometheus-operator/prometheus-operator#prometheus-operator) to manage the life-cycle of Prometheus and AlertManager custom resource definitions (CRDs)
 * [Promscale](https://github.com/timescale/promscale) ([design doc][design-doc]) to store metrics for the long-term and allow analysis with both PromQL and SQL.
 * [TimescaleDB](https://github.com/timescale/timescaledb) for long term storage of metrics and provides ability to query metrics data using SQL. 
 * [Promlens](https://promlens.com/) tool to build and analyse promql queries with ease.
 * [Opentelemetry-Operator](https://github.com/open-telemetry/opentelemetry-operator#opentelemetry-operator-for-kubernetes) to manage the lifecycle of OpenTelemetryCollector Custom Resource Definition (CRDs)
 * [Jaeger Query](https://github.com/jaegertracing/jaeger) to visualise the traces 
 
We plan to expand this stack over time and welcome contributions.

Tobs provides a CLI tool to make deployment and operations easier. We also provide
Helm charts that can be used directly or as sub-charts for other projects.

## 🔥 Quick start

### Installing the CLI tool

To download and install tobs, run the following in your terminal, then follow the on-screen instructions.

```bash
curl --proto '=https' --tlsv1.2 -sSLf  https://tsdb.co/install-tobs-sh |sh
```

Alternatively, you can download the CLI directly via [our releases page](https://github.com/timescale/tobs/releases/latest)

Getting started with the CLI tool is a two-step process: First you install the CLI tool locally, then you use the CLI tool to install the tobs stack into your Kubernetes cluster.

### Using the tobs CLI tool to deploy the stack into your Kubernetes cluster

After setting up tobs run the following to install the tobs helm charts into your Kubernetes cluster

```bash
tobs install
```

This will deploy all of the tobs components into your cluster and provide instructions as to next steps.

#### Tracing support

From `0.7.0` release tobs supports installation of tracing components. To install tracing components use 
```
tobs install --tracing
```
For more details on tracing support visit [Promscale tracing docs](https://github.com/timescale/promscale/blob/master/docs/tracing.md).

### Using the tobs CLI tool

The [usage guide][tobs-cli-usage-guide] for CLI tool on tobs github repo provides the most seamless experience for interacting with tobs.

## Configuring the stack

All configuration for all components happens through the helm values.yml file.
You can view the self-documenting [default values.yaml][tobs-helm-values] in the tobs repo.
We also have additional documentation about individual configuration settings in our
[Helm chart docs][[tobs-helm-values-config-details]].

To modify the settings, first create a values.yaml file:
```bash
tobs helm show-values > values.yaml
```

Then modify the values.yaml file using your favorite editor.
Finally, deploy with the new settings using:
```bash
tobs install -f values.yaml
```

## 🛠 Using the Tobs Helm chart without the CLI tool [](tobs-helm-installation)

Users sometimes want to use our Helm charts as sub-charts for other project or integrate them into their infrastructure without using our CLI tool. Below are instruction to install tobs using helm chart.

### Installing the helm chart

The following command will install Kube-Prometheus, TimescaleDB, and Promscale
into your Kubernetes cluster:
```
helm repo add timescale https://charts.timescale.com/
helm repo update
helm install <release_name> timescale/tobs
```

**Note**: To customise the tobs helm chart installtion you can use tobs [values.yaml][tobs-helm-values] with your desired changes. 

## Compatibility matrix

## Tobs vs. Kubernetes

| Tobs     | Kubernetes           |
|----------|----------------------|
| 0.7.0    | v1.19 to v1.21       | 


### Next Steps
Congratulations, you've successfully deployed the observability stack using tobs. You can explore your data by connecting to your TimescaleDB instance and visualise data using Grafana.

To continue your Promscale learning journey, check out:

* [Promscale overview video][promscale-overview-video]
* [Promscale Features](<LINK_TO_PROMSCALE_FEATURES_PAGE> TODO)
* [Run queries with PromQL and SQL][promscale-run-queries]

[promscale-run-queries]: /tutorials/promscale/promscale-run-queries/
[promscale-overview-video]: https://youtu.be/FWZju1De5lc
[tobs-helm-values]: https://github.com/timescale/tobs/blob/master/chart/values.yaml
[tobs-helm-values-config-details]: https://github.com/timescale/tobs/tree/master/chart#configuring-helm-chart
[tobs-cli-usage-guide]: https://github.com/timescale/tobs/tree/master/cli#usage-guide