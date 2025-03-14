# grafana-dashboards-kubernetes <!-- omit in toc -->

![logo](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/kubernetes-grafana-dashboards-logo.png)

## Table of contents <!-- omit in toc -->

- [Description](#description)
- [Features](#features)
- [Dashboards](#dashboards)
- [Installation](#installation)
  - [Install manually](#install-manually)
  - [Install via grafana.com](#install-via-grafanacom)
  - [Install with ArgoCD](#install-with-argocd)
  - [Install as ConfigMaps](#install-as-configmaps)
  - [Install as ConfigMaps with Terraform](#install-as-configmaps-with-terraform)
- [Contributing](#contributing)

## Description

This repository contains a modern set of [Grafana](https://github.com/grafana/grafana) dashboards for [Kubernetes](https://github.com/kubernetes/kubernetes).\
They are inspired by many other dashboards from `kubernetes-mixin` and `grafana.com`.

More information about them in my article: [A set of modern Grafana dashboards for Kubernetes](https://0xdc.me/blog/a-set-of-modern-grafana-dashboards-for-kubernetes/)

You can also download them on [Grafana.com](https://grafana.com/grafana/dashboards/?plcmt=top-nav&cta=downloads&search=dotdc).

## Features

Theses dashboards are made and tested for the [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) chart, but they should work well with others as soon as you have [kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) and [prometheus-node-exporter](https://github.com/prometheus/node_exporter) installed on your Kubernetes cluster.

They are not backward compatible with older Grafana versions because they try to take advantage of Grafana's newest features like:

- `gradient mode` introduced in Grafana 8.1 ([Grafana Blog post](https://grafana.com/blog/2021/09/10/new-in-grafana-8.1-gradient-mode-for-time-series-visualizations-and-dynamic-panel-configuration/))
- `time series` visualization panel introduced in Grafana 7.4 ([Grafana Blog post](https://grafana.com/blog/2021/02/10/how-the-new-time-series-panel-brings-major-performance-improvements-and-new-visualization-features-to-grafana-7.4/))
- `$__rate_interval` variable introduced in Grafana 7.2 ([Grafana Blog post](https://grafana.com/blog/2020/09/28/new-in-grafana-7.2-__rate_interval-for-prometheus-rate-queries-that-just-work/))

They also have a `Prometheus Datasource` variable so they will work on a federated Grafana instance.

As an example, here's how the `Kubernetes / Views / Global` dashboard looks like:

![screenshot](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-global.png "Kubernetes Global View Screenshot")

## Dashboards

| File name                  | Description | Screenshot |
|:---------------------------|:------------|:----------:|
| k8s-addons-starboard-operator.json | Dashboard for the Starboard Operator from Aqua Security. | [LINK](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-addons-starboard-operator.png) |
| k8s-system-api-server.json | Dashboard for the API Server Kubernetes. | [LINK](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-system-api-server.png) |
| k8s-system-coredns.json    | Show information on the CoreDNS Kubernetes component. | [LINK](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-system-coredns.png) |
| k8s-views-global.json      | `Global` level view dashboard for Kubernetes. | [LINK](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-global.png) |
| k8s-views-namespaces.json  | `Namespaces` level view dashboard for Kubernetes. | [LINK](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-namespaces.png) |
| k8s-views-nodes.json       | `Nodes` level view dashboard for Kubernetes. | [LINK](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-nodes.png) |
| k8s-views-pods.json        | `Pods` level view dashboard for Kubernetes. | [LINK](https://raw.githubusercontent.com/dotdc/media/main/grafana-dashboards-kubernetes/k8s-views-pods.png) |

## Installation

In most installation cases, you will need to clone this repository (or your fork):

```terminal
git clone https://github.com/dotdc/grafana-dashboards-kubernetes.git
cd grafana-dashboards-kubernetes
```

### Install manually

On the WebUI of your Grafana instance, put your mouse over the `+` sign on the left menu, then click on `Import`.\
Once you are on the Import page, you can upload the JSON files one by one from your local copy using the `Upload JSON file` button.

### Install via grafana.com

On the WebUI of your Grafana instance, put your mouse over the `+` sign on the left menu, then click on `Import`.\
Once you are on the Import page, you can put the grafana.com dashboard id (see table bellow) under `Import via grafana.com` then click on the `Load` button. Repeat for each dashboard.

Grafana.com dashboard id list:

| Dashboard                          | ID    |
|:-----------------------------------|:------|
| k8s-addons-starboard-operator.json | 16337 |
| k8s-system-api-server.json         | 15761 |
| k8s-system-coredns.json            | 15762 |
| k8s-views-global.json              | 15757 |
| k8s-views-namespaces.json          | 15758 |
| k8s-views-nodes.json               | 15759 |
| k8s-views-pods.json                | 15760 |

### Install with ArgoCD

If you have ArgoCD, this will deploy the dashboards in ArgoCD's default project:

```terminal
kubectl apply -f argocd-app.yml
```

### Install as ConfigMaps

Grafana dashboards can be provisionned as Kubernetes ConfigMaps if you configure the [dashboard sidecar](https://github.com/grafana/helm-charts/blob/main/charts/grafana/values.yaml#L667) available on the official [Grafana Helm Chart](https://github.com/grafana/helm-charts/tree/main/charts/grafana).

To build the ConfigMaps and output them on STDOUT :

```terminal
kubectl kustomize .
```

*Note: no namespace is set by default, you can change that in the `kustomization.yaml` file.*

To build and deploy them directly on your kubernetes cluster :

```terminal
kubectl apply -k . -n monitoring
```

*Note: you can change the namespace if needed.*

### Install as ConfigMaps with Terraform

If you use terraform to provision your Kubernetes resources, you can convert the generated ConfigMaps to Terraform code using [tfk8s](https://github.com/jrhouston/tfk8s).

To build and convert ConfigMaps to Terraform code :

```terminal
kubectl kustomize . | tfk8s
```

*Note: no namespace is set by default, you can change that in the `kustomization.yaml` file.*

## Contributing

Feel free to contribute to this project:

- Give a GitHub ⭐ if you like it
- Create an [Issue](https://github.com/dotdc/grafana-dashboards-kubernetes/issues) to make a feature request, report a bug or share an idea.
- Create a [Pull Request](https://github.com/dotdc/grafana-dashboards-kubernetes/pulls) if you want to share code or anything useful to this project.
