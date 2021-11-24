# Promscale instalaltion using helm

You can install Prometheus and Promscale using helm. But this requires using multiple helm charts. As Prometheus, Promscale and TimescaleDB are three different helm charts.

Below are steps to install all the components using helm charts

1. [Install TimescaleDB helm chart][install-timescaledb]
2. [Install Promscale helm chart][install-promscale]
3. [Install Prometheus helm chart][install-prometheus]

Alternatively, You can use single helm chart to install all the above listed components using [tobs helm chart][tobs-helm-chart-install]. 

## Install TimescaleDB [](install-timescaledb)

To install the chart with the release name `my-release`, first  in `values.yaml` you need to set credentials mentioned in list
below. If you decide not to set those credentials, they will be randomly generated. Those credentials can be setup via helm only
during helm first run and they won't be rotated with subsequent helm update commands to prevent breaking the database.

* The credentials for the superuser, admin and stand-by users
* TLS Certificates
* pgbackrest config (optional)

Then you can install the chart with:
```console
helm install --name my-release charts/timescaledb-single
```

You can override parameters using the `--set key=value[,key=value]` argument to `helm install`,
e.g., to install the chart with backup enabled:

```console
helm install --name my-release charts/timescaledb-single --set backup.enabled=true
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,
```console
helm install --name my-release -f myvalues.yaml charts/timescaledb-single
```

For details about what parameters you can set, have a look at the [Administrator Guide][timescaledb-helm-chart-admin-guide]

### Installing from the Timescale Helm Repo

We have a Helm Repository that you can use, instead of cloning this Git repo. 

First add the repository with:
```console
helm repo add timescale 'https://charts.timescale.com'
```
> **NOTICE**: Helm chart installation will randomly generate secrets which cannot be rotated with subsequent helm upgrades.
If you want to use predefined credentials, please set them in `secrets` section of `values.yaml` before running `helm install`.

Next proceed to install the chart:

```console
helm install my-release .
```

To keep the repo up to date with new versions you can do:
```console
helm repo update
``` 

## Install Promscale [](install-promscale)

### Prerequisites

For promscale to work correctly it needs a set of data to connect to timescale database. This 
configuration can be supplied in two ways either by using DB URI or by specifying connection
parameters. Options are mutually exclusive and specifying URI takes priority.

### Using DB URI

You can use db uri to connect to TimescaleDB. To do so, specify the URI in values.yaml as follows:
```yaml
connection:
  uri: <TIMESCALE_DB_URI>
```

### Using Connection Parameters

Instead of using db uri, you can specify all parameters necessary for connecting promscale to timescaledb using `connection` map.
Bear in mind that timescale database should exist before starting promscale or at least credentials should be available.

Following are the default configuration values:

```yaml
connection:
  user: postgres
  password: ""
  host: db.timescale.svc.cluster.local
  port: 5432
  sslMode: require
  dbName: timescale
```

### Installation

To install the chart with the release name `my-release`:
```shell script
helm install --name my-release .
```

You can override parameters using the `--set key=value[,key=value]` argument
to `helm install`, e.g. to install the chart with specifying a previously created
secret `timescale-secret` and an existing TimescaleDB instance:
```shell script
helm install --name my-release . \
      --set connection.password.secretTemplate="timescale-secret"
      --set connection.host.nameTemplate="timescaledb.default.svc.cluster.local"
```

You can also install by referencing the db uri secret:

```shell script
helm install --name my-release . \
      --set connection.dbURI.secretTemplate="timescale-secret"
```
 
Alternatively, a YAML file the specifies the values for the parameters can be provided
while installing the chart. For example:
```shell script
helm install --name my-release -f myvalues.yaml .
```

## Install Prometheus [](install-prometheus)

This chart bootstraps a [Prometheus][prometheus-website] deployment on a [Kubernetes][k8s-website] cluster using the [Helm][helm-website] package manager.

### Prerequisites

- Kubernetes 1.16+
- Helm 3+

### Get Repo Info

```console
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

### Install Chart

```console
helm install [RELEASE_NAME] prometheus-community/prometheus
```

_See detailed helm chart configuration at [prometheus helm chart][prometheus-helm-chart-config]._


### Dependencies

By default this chart installs additional, dependent charts:

- [kube-state-metrics][kube-state-metrics-gh]

To disable the dependency during installation, set `kubeStateMetrics.enabled` to `false`.

<ADD_BREAK_SEGMENT_LINE_TO_DISTINGUISH_THE_NEXT_STEPS_SECTION_FROM_THIS_SECTION>

### Next Steps
Congratulations, you've successfully deployed Promscale and Prometheus using helm. You can explore your data by connecting to your TimescaleDB instance.

To continue your Promscale learning journey, check out:

* [Promscale overview video][promscale-overview-video]
* [Promscale Features](<LINK_TO_PROMSCALE_FEATURES_PAGE> TODO)
* [Run queries with PromQL and SQL][promscale-run-queries]

[promscale-run-queries]: /tutorials/promscale/promscale-run-queries/
[promscale-overview-video]: https://youtu.be/FWZju1De5lc
[tobs-helm-chart-install]: /tutorials/promscale/installation/tobs/#tobs-helm-installation
[timescaledb-helm-chart-admin-guide]: https://github.com/timescale/timescaledb-kubernetes/blob/master/charts/timescaledb-single/admin-guide.md#configuration
[prometheus-website]: https://prometheus.io/
[k8s-website]: (http://kubernetes.io)
[helm-website]: https://helm.sh
[prometheus-helm-chart-config]: https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus#configuration
[kube-state-metrics-gh]: https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-state-metrics