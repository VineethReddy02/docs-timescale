# Up and running: Install Prometheus, Promscale, and TimescaleDB

## Installation Pre-requisites

### New installation with Prometheus

1. As the first step in getting started with monitoring. You need to install Prometheus. Prometheus can be deployed as standalone process in bare-metal VM's, as a container in containerised environment and as a deployment, statefulset in Kubernetes. Prometheus installation steps can be found [here][prometheus-installation]
2. Now install the Promscale as mentioned [here][#install-promscale]   

### Installing Promscale to your existing monitoring solution

1. Integrating to Prometheus or Replacing other remote-write systems with Promscale

If you are already using Prometheus or an other remote storage system for long term storage of metrics and planning to replace remote-storage system with Promscale. We have tool to acheieve this using **Prom-migrator**, Prom-migrator is an open-source, community-driven and free-to-use, universal prometheus data migration tool, that migrates data from one storage system to another, leveraging Prometheus's remote storage endpoints. 

Follow the below steps to migrate the existing data into Promscale:

1. Install the Promscale as described [here][#install-promscale] 

2. Install the Prom-migrator from [Promscale releases][promscale-gh-releases].

3. Now the run the Prom-migrator to copy the data from existing Prometehues or remote-storage system to Promscale using the command:
```yaml
./prom-migrator -start=<migration-data-start-time> -end=<migration-data-end-time> -reader-url=<read_endpoint_url_for_remote_read_storage> -writer-url=<write_endpoint_url_for_remote_write_storage> -progress-metric-url=<read_endpoint_url_for_remote_write_storage>
```
More details on Prom-migrator and it's CLI flags can be found [here][prom-migrator-readme].

4. Now after successfully migrating the data into Promscale you can drop the old data from Prometheus and the other remote-storage system as all this data is copied into Promscale using Prom-migrator from the previous step. 

5. Currently. Promscale doesn't support alerting, recording rules. So this needs to be achieved on Prometheus end by configuring the alerting, recording rules and an alert-manager to fire alerts. We recommend to configure the data renetion in Prometheus to `1d` but this is totally dependent on the alerting rules configured in Prometheus for evaluation. In future versions of Promscale we will support this natively in Promscale.

6. By now all your data should be persisted into Promscale!! Now you can directly query from Promscale using PromQL and query using SQL from TimescaleDB. 

## Installation Methods [](install-promscale)

We recommend four methods to setup and install Promscale:
1. [TOBS - The Observability Stack for Kubernetes ][tobs-install] (recommended for kubernetes deployments)
2. [Docker][docker-install] 
3. [Helm][helm-chart-install]
4. [Bare Metal][bare-metal-install]

[prometheus-installation]: https://prometheus.io/docs/prometheus/latest/installation/
[promscale-gh-releases]: https://github.com/timescale/promscale/releases
[tobs-install]: /installation/tobs
[docker-install]: /installation/docker
[helm-chart-install]: /installation/helm
[bare-metal-install]: /installation/bare-metal