# Palworld Helm Chart

[![GitHub Release](https://img.shields.io/github/v/release/korax-dev/helm-charts?filter=palworld*)](https://github.com/korax-dev/helm-charts/releases)

This chart installs [docker-palworld-dedicated-server](https://github.com/jammsen/docker-palworld-dedicated-server) on a [Kubernetes](http://kubernetes.io/) cluster using the [Helm](https://helm.sh/) package manager.

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

To install the palworld chart:

```sh
helm install palworld korax-dev/palworld
```

To upgrade the palworld chart:

```sh
helm upgrade palworld korax-dev/palworld
```

To uninstall the chart:

```sh
helm uninstall palworld
```

## Configuration

See [Customizing the Chart Before Installing](https://helm.sh/docs/intro/using_helm/#customizing-the-chart-before-installing). To see all configurable options with detailed comments, visit the chart's [values.yaml](./values.yaml).

### Networking

This Chart uses a [LoadBalancer Service](https://kubernetes.io/docs/concepts/services-networking/service/#loadbalancer) to expose the application outside Kubernetes:

```yaml
service:
  type: LoadBalancer
  ## Example annotations for kube-vip or MetalLB to assign an IP to the LoadBalancer
  annotations: {}
    # kube-vip.io/loadbalancerIPs: 192.168.1.100
    # metallb.universe.tf/loadBalancerIPs: 192.168.1.100
  port: 8211
```

Note: Port-Forwarding may be required if the LoadBalancer does not use a Public IP.

### Persistent Volume

Data persistance is enabled by default using dynamic volume provisioning.

> [!WARNING]
> Set the configured storageClass [reclaim policy](https://kubernetes.io/docs/concepts/storage/storage-classes/#reclaim-policy) to `Retain` to prevent automatic deletion of the PV if the chart is uninstalled.

### Palworld Config

To configure the server, add the [environment variables](https://github.com/jammsen/docker-palworld-dedicated-server/blob/develop/docs/ENV_VARS.md) to `config`:

```yaml
config:
  env:
    TZ: UTC                             # Timezone used for time stamping server backups
    ALWAYS_UPDATE_ON_START: true
    MULTITHREAD_ENABLED: true
    COMMUNITY_SERVER: false
    BACKUP_ENABLED: true
    BACKUP_CRON_EXPRESSION: "0 * * * *"  # Backup every hour
    BACKUP_RETENTION_POLICY: true        # Cleanup old backups
    BACKUP_RETENTION_AMOUNT_TO_KEEP: 168 # Retain backups for 7 days
    SERVER_SETTINGS_MODE: auto           # Apply game settings using environment variables
    SERVER_NAME: "Default Palworld Server"
    SERVER_DESCRIPTION: ""
    MAX_PLAYERS: 32
    PUBLIC_IP: ""
    PUBLIC_PORT: 8211
  
  secretEnv:
    create: true
    env:
      SERVER_PASSWORD: "serverPasswordHere"
      ADMIN_PASSWORD: "adminPasswordHere"
```

### Prometheus Exporter

This chart supports exporting Prometheus metrics with [palworld-exporter](https://github.com/palworldlol/palworld-exporter). To enable metrics, set the following values:

```yaml
metrics:
  enabled: true

  serviceMonitor:
    ## Create Prometheus Operator ServiceMonitor resource
    enabled: true

  ## Additional environment variables used to configure palworld-exporter
  ## ref: https://github.com/palworldlol/palworld-exporter/tree/main?tab=readme-ov-file#options
  ##
  env:
    ## Path to your Palworld save directory
    SAVE_DIRECTORY: /palworld/Pal/Saved/SaveGames/0/XXXXX.../
```

> [!NOTE]
> RCON must be enabled to use palworld-exporter.

See the [palworld-exporter documentation](https://github.com/palworldlol/palworld-exporter) for additional details and an example Grafana dashboard.
