# Sons of the Forest Helm Chart

[![GitHub Release](https://img.shields.io/github/v/release/korax-dev/helm-charts?filter=sons-of-the-forest*)](https://github.com/korax-dev/helm-charts/releases)

This chart installs [docker-sons-of-the-forest-dedicated-server](https://github.com/jammsen/docker-sons-of-the-forest-dedicated-server) on a [Kubernetes](http://kubernetes.io/) cluster using the [Helm](https://helm.sh/) package manager.

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
helm install sons-of-the-forest korax-dev/sons-of-the-forest
```

To upgrade the sons-of-the-forest chart:

```sh
helm upgrade sons-of-the-forest korax-dev/sons-of-the-forest
```

To uninstall the chart:

```sh
helm uninstall sons-of-the-forest
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
  gamePort: 8766
  queryPort: 27016
  blobSyncPort: 9700
```

Note: Port-Forwarding may be required if the LoadBalancer does not use a Public IP.

### Persistent Volume

Data persistance is enabled by default using dynamic volume provisioning.

> [!WARNING]
> Set the configured storageClass [reclaim policy](https://kubernetes.io/docs/concepts/storage/storage-classes/#reclaim-policy) to `Retain` to prevent automatic deletion of the PV if the chart is uninstalled.

### Server configuration

To configure the server, add the settings to `serverConfig`:

```yaml
serverConfig:
  IpAddress: "0.0.0.0"
  GamePort: 8766
  QueryPort: 27016
  BlobSyncPort: 9700
  ServerName: "Sons of the Forest Server (dedicated)"
  MaxPlayers: 8
  Password: ""
  LanOnly: false
  SaveSlot: 1
  SaveMode: "Continue"
  GameMode: "Normal"
  SaveInterval: 600
  IdleDayCycleSpeed: 0.0
  IdleTargetFramerate: 5
  ActiveTargetFramerate: 60
  LogFilesEnabled: false
  TimestampLogFilenames: true
  TimestampLogEntries: true
  GameSettings: {}
  CustomGameModeSettings: {}
```

In order to be able to administrate your server from in game directly, you will
need add your SteamID to `ownersWhitelist`. To find your SteamID, open Steam and
click on your name on the top right, then go to Account Details.

```yaml
ownersWhitelist: |
  # In order to be able to administrate your server from in game directly, you will need to setup server ownership.
  # Add below the steam ids of every server owner, one steam id per line.
  # To find your SteamID, open Steam and click on your name on the top right, then go to Account Details.

  YOUR_STEAM_ID
```

> [!TIP]
> See the Sons of the Forest [Dedicated Server Configuration Guide](https://steamcommunity.com/sharedfiles/filedetails/?id=2992700419) for an overview of the various configuration settings.
