# lock-unlock-subgraph

![Version: 0.0.1](https://img.shields.io/badge/Version-0.0.1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 0.1.0](https://img.shields.io/badge/AppVersion-0.1.0-informational?style=flat-square)

Helm charts for the Fuseki deployment of Lock-Unlock with Subgraph strategy

**Homepage:** <https://labs.kadaster.nl/cases/lockunlock>

## Source Code

* <https://github.com/kadaster-labs/secured-sparql-endpoint-subgraph>

## Get Repo Info
```console
helm repo add lock-unlock https://kadaster-labs.github.io/lock-unlock-helm-charts/
helm repo update
```

## Install Chart
```console
helm install [RELEASE_NAME] lock-unlock/lock-unlock-subgraph
```

The command deploys lock-unlock-subgraph on the Kubernetes cluster in the default configuration.

> [!IMPORTANT]
> The Helm chart doesn't work as is, as the default container registry isn't publicly accessible. Please contact our team to get information about the required `imagePullSecrets` configuration, or build the images yourself. The charts accommodate setting your own registry.

_See [configuration](#configuration) below._

## Uninstall Chart
```console
helm uninstall [RELEASE_NAME]
```

This removes all the Kubernetes components associated with the chart and deletes the release.

## Upgrading Chart
```console
helm upgrade [RELEASE_NAME] [CHART] --install
```

## Configuration
To see all configurable options with detailed comments, visit the chart's values.yaml, or run these configuration commands:

```console
helm show values lock-unlock/lock-unlock-subgraph
```

It is required to either set the `imagePullSecrets`, or build the images yourself and supply your own container registry. Instructions on how to create a Secret by providing credentials on the command line can be found [here](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-secret-by-providing-credentials-on-the-command-line).

### Resources
The Java VM requires at least 2Gi of memory to function properly, so the Helm deployment uses this as default. You may change this value as desired.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| autoscaling.enabled | bool | `false` |  |
| autoscaling.targetCPUUtilizationPercentage | int | `80` |  |
| image.javaOptions | string | `"-Xmx8g -Xms4g"` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"lockunlock.azurecr.io/subgraph"` |  |
| image.resources.limits.memory | string | `"8Gi"` |  |
| image.resources.requests.cpu | string | `"50m"` |  |
| image.resources.requests.memory | string | `"2Gi"` |  |
| image.tag | string | `"0.0.1"` |  |
| ingress.enabled | bool | `false` |  |
| ingress.hosts[0].host | string | `"chart-example.local"` |  |
| ingress.hosts[0].paths[0].path | string | `"/"` |  |
| ingress.hosts[0].paths[0].pathType | string | `"ImplementationSpecific"` |  |
| replicaCount | int | `1` |  |
| service.port | int | `80` |  |
| service.type | string | `"ClusterIP"` |  |
| serviceAccount.automount | bool | `true` |  |
| serviceAccount.create | bool | `true` |  |
