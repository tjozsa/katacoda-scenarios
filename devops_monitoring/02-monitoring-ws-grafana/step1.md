## Your Prometheus and Grafana environment is starting
Give it some time to load all the images and start up. It is happening as you read these lines automatically.

## Meet Grafana

We have added a new container into the `workshop-prometheus-grafana/docker-compose.yml`{{open}}. Check and discuss the changes.

You can log into Grafana here:
https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com/

user: **grep** / password: **demo**

Explore Grafana.

Add a new datasource to Grafana.

- Mode: `server`
- Pointing to `http://prometheus:9090`{{copy}}

![](assets/grafana-setup-datasource.png)

## Add a custom dashboard

Add a new dashboard to Grafana.

### Simple panel / graph

Create a graph showing current memory usage.

<details>
  <summary>💡 Solution</summary>

  Query: `(node_memory_MemTotal_bytes{} - node_memory_MemFree_bytes{}) / node_memory_MemTotal_bytes{} * 100`

  ![](assets/grafana-new-metric.png)
</details>

### Some formatting

Grafana should be displaying graph in %, such as:

![](assets/grafana-graph-percent.png)

<details>
  <summary>💡 Solution</summary>

  ![](assets/grafana-set-unit.png)
</details>

### CPU load

In the same dashboard, add a new graph for CPU load (1min, 5min, 15min).

Tips: you will need a new metric prefixed by `node_`.

![](assets/grafana-cpu-load.png)

<details>
  <summary>💡 Solution</summary>

  ![](assets/grafana-set-cpu-load-metrics.png)
</details>

### Disk usage

In the same dashboard, add a new graph for `sda` disk usage (ko written per second).

You will need `rate()` PromQL function: [https://prometheus.io/docs/prometheus/latest/querying/functions/#rate](https://prometheus.io/docs/prometheus/latest/querying/functions/#rate)

![](assets/grafana-disk-load.png)

<details>
  <summary>💡 Solution</summary>

  Query:
  `rate(node_disk_written_bytes_total{device="sda"}[30s])`

</details>

## Dashboards from community

Let's import a dashboard from Grafana website.

- "Node Exporter Full" dashboard: [https://grafana.com/dashboards/1860](https://grafana.com/dashboards/1860)
- Or "Node Exporter Server Metrics" dashboard: [https://grafana.com/dashboards/405](https://grafana.com/dashboards/405)
- Or both ;)

Those dashboards are only compatible with Prometheus data-source and node-exporter.

![](assets/grafana-community-dash.png)

