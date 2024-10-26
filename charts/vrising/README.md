# V Rising Helm Chart

[![GitHub Release](https://img.shields.io/github/v/release/korax-dev/helm-charts?filter=vrising*)](https://github.com/korax-dev/helm-charts/releases)

This chart installs [docker-vrising](https://github.com/TrueOsiris/docker-vrising) on a [Kubernetes](http://kubernetes.io/) cluster using the [Helm](https://helm.sh/) package manager.

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
helm install vrising korax-dev/vrising
```

To upgrade the vrising chart:

```sh
helm upgrade vrising korax-dev/vrising
```

To uninstall the chart:

```sh
helm uninstall vrising
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
  gamePort: 9876
  queryPort: 9877
  rconPort: 25575
```

Note: Port-Forwarding may be required if the LoadBalancer does not use a Public IP.

### Persistent volume

Data persistance is enabled by default using dynamic volume provisioning.

> [!WARNING]
> Set the configured storageClass [reclaim policy](https://kubernetes.io/docs/concepts/storage/storage-classes/#reclaim-policy) to `Retain` to prevent automatic deletion of the PV if the chart is uninstalled.

### Server configuration

To configure the server, add the settings under the `serverHostSettings` key. See <https://github.com/StunlockStudios/vrising-dedicated-server-instructions/blob/master/1.0.x/INSTRUCTIONS.md> for reference.

```yaml
serverHostSettings:
  Name: V Rising Server
  Description: ''
  Port: 9876
  QueryPort: 9877
  MaxConnectedUsers: 40
  MaxConnectedAdmins: 4
  ServerFps: 30
  SaveName: world1
  Password: ''
  Secure: true
  ListOnSteam: false
  ListOnEOS: false
  AutoSaveCount: 20
  AutoSaveInterval: 120
  CompressSaveFiles: true
  GameSettingsPreset: ''
  GameDifficultyPreset: ''
  AdminOnlyDebugEvents: true
  DisableDebugEvents: false
  API:
    Enabled: false
  Rcon:
    Enabled: false
    Port: 25575
    Password: ''
```

Additional game settings can be applied under the `serverGameSettings` key. See <https://cdn.stunlock.com/blog/2022/05/25083113/Game-Server-Settings.pdf> for reference.

> [!TIP]
> Settings are converted to json and saved directly to the `ServerHostSettings.json` and `ServerGameSettings.json` files. Additional settings that are not found in this Chart can still be applied using their respective keys.

### Admin list

To grant admin access to the server, place [SteamIDs](https://help.steampowered.com/en/faqs/view/2816-BE67-5B69-0FEC) under the `adminList` key.

```yaml
# Example
adminList:
  - "76561197#########"
  - "76561198#########"
```
