---
title: Integrating Longhorn metrics into the Rancher monitoring system
weight: 2
---
## About Rancher monitoring system

Using Rancher, you can monitor the state and processes of your cluster nodes,
Kubernetes components,
and software deployments through integration with [Prometheus](https://prometheus.io/),
a leading open-source monitoring solution.

Using Prometheus, you can monitor Rancher at both the cluster level and the project level.
For each cluster and project that is enabled for monitoring, Rancher deploys a Prometheus server.

The [Rancher monitoring system](https://rancher.com/docs/rancher/v2.x/en/project-admin/tools/monitoring/) automatically deploys Alertmanager and Grafana for you.

## Add Longhorn metrics to Rancher monitoring system

If you are using Rancher to manage your Kubernetes and already enabled Rancher monitoring, you can add Longhorn metrics to Rancher monitoring by simply deploying the following ServiceMonitor:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: longhorn-prometheus-servicemonitor
  namespace: longhorn-system
  labels:
    name: longhorn-prometheus-servicemonitor
spec:
  selector:
    matchLabels:
      app: longhorn-manager
  namespaceSelector:
    matchNames:
    - longhorn-system
  endpoints:
  - port: manager
```

Once the ServiceMonitor is created, Rancher will automatically discover all Longhorn metrics.

You can then set up a [Grafana](https://rancher.com/docs/rancher/v2.x/en/cluster-admin/tools/monitoring/viewing-metrics/#grafana) dashboard for visualization.
You can import our prebuilt [Longhorn example dashboard](https://grafana.com/grafana/dashboards/13032) to have an idea.

You can also [set up alerts](https://rancher.com/docs/rancher/v2.x/en/cluster-admin/tools/alerts/) in Rancher UI.