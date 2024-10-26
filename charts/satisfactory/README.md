# Satisfactory Helm Chart

[![GitHub Release](https://img.shields.io/github/v/release/korax-dev/helm-charts?filter=satisfactory*)](https://github.com/korax-dev/helm-charts/releases)

This chart installs [satisfactory-server](https://github.com/wolveix/satisfactory-server) on a [Kubernetes](http://kubernetes.io/) cluster using the [Helm](https://helm.sh/) package manager.

## Usage

[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

```sh
helm repo add korax-dev https://korax-dev.github.io/helm-charts
```

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo
korax-dev` to see the charts.

To install the  chart:

```sh
helm install satisfactory korax-dev/satisfactory
```

To upgrade the satisfactory chart:

```sh
helm upgrade satisfactory korax-dev/satisfactory
```

To uninstall the chart:

```sh
helm uninstall satisfactory
```

## Configuration

See [Customizing the Chart Before Installing](https://helm.sh/docs/intro/using_helm/#customizing-the-chart-before-installing). To see all configurable options with detailed comments, visit the chart's [values.yaml](./values.yaml).

### Networking

This chart uses a [LoadBalancer Service](https://kubernetes.io/docs/concepts/services-networking/service/#loadbalancer) to expose the application outside Kubernetes:

```yaml
service:
  type: LoadBalancer
  ## Example annotations for kube-vip or MetalLB to assign an IP to the LoadBalancer
  annotations: {}
    # kube-vip.io/loadbalancerIPs: 192.168.1.100
    # metallb.universe.tf/loadBalancerIPs: 192.168.1.100
  gamePort: 7777
```

Note: Port-Forwarding may be required if the LoadBalancer does not use a Public IP.

### Persistent volume

Data persistance is enabled by default using dynamic volume provisioning.

> [!WARNING]
> Set the configured storageClass [reclaim policy](https://kubernetes.io/docs/concepts/storage/storage-classes/#reclaim-policy) to `Retain` to prevent automatic deletion of the PV if the chart is uninstalled.
