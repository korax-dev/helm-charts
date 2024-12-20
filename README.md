# Dedicated Server Helm Charts

This repository contains Helm charts for the following dedicated servers:

- [Palworld](./charts/palworld/)
- [Satisfactory](./charts/satisfactory/)
- [Sons of the Forest](./charts/sons-of-the-forest)
- [V Rising](./charts/vrising)

See the respective links for documentation related to that chart.

## Usage

[Helm](https://helm.sh) must be installed to use the charts. Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

```sh
helm repo add korax-dev https://korax-dev.github.io/helm-charts
```

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages. You can then run `helm search repo
korax-dev` to see the charts.
