# lock-unlock-rewrite

![Version: 0.1.1](https://img.shields.io/badge/Version-0.1.1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 0.1.0](https://img.shields.io/badge/AppVersion-0.1.0-informational?style=flat-square)

Helm charts for the Fuseki deployment of Lock-Unlock with Rewrite strategy

**Homepage:** <https://labs.kadaster.nl/cases/lockunlock>

## Source Code

* <https://github.com/kadaster-labs/secured-sparql-endpoint-rewrite>

## Get Repo Info
```console
helm repo add lock-unlock https://kadaster-labs.github.io/lock-unlock-helm-charts/
helm repo update
```

## Install Chart
```console
helm install [RELEASE_NAME] lock-unlock/lock-unlock-rewrite
```

The command deploys lock-unlock-rewrite on the Kubernetes cluster in the default configuration.

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

helm show values lock-unlock/lock-unlock-rewrite

### Dataloader
The [dataloader](https://github.com/kadaster-labs/lock-unlock-testdata/tree/main/lock-unlock-dataloader) runs as an initContainer, during which it prepares the Fuseki database.

> [!IMPORTANT]
> The `dataloader.dataset.file_url` setting is required in order for this deployment to function. It does not work without supplying dataset to host.

An example location for a dataset is https://raw.githubusercontent.com/kadaster-labs/lock-unlock-testdata/main/testdata-$DATASET/lock-unlock-$DATASET.ttl.gz, where acceptable values for DATASET are: `anbi`, `brk`, `brp` and `nhr`.

### Resources
The Java VM requires at least 2Gi of memory to function properly, so you might want to set the image to have the following resources:

```yaml
resources:
  limits:
    memory: 2Gi
  requests:
    cpu: 50m
    memory: 2Gi
```

Adjust as desired.

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| dataloader.dataset.endpoint | string | `"/brk"` |  |
| dataloader.dataset.file_url | string | `"https://raw.githubusercontent.com/kadaster-labs/lock-unlock-testdata/main/testdata-brk/lock-unlock-brk.ttl.gz"` |  |
| dataloader.enabled | bool | `true` |  |
| dataloader.image.pullPolicy | string | `"IfNotPresent"` |  |
| dataloader.image.repository | string | `"lockunlock.azurecr.io/dataloader"` |  |
| dataloader.image.tag | string | `"0.1.0"` |  |
| dataloader.resources | object | `{}` |  |
| fullnameOverride | string | `""` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"lockunlock.azurecr.io/rewrite"` |  |
| image.tag | string | `"0.1.0"` |  |
| imagePullSecrets | list | `[]` |  |
| ingress.annotations | object | `{}` |  |
| ingress.className | string | `""` |  |
| ingress.enabled | bool | `false` |  |
| ingress.hosts[0].host | string | `"chart-example.local"` |  |
| ingress.hosts[0].paths[0].path | string | `"/"` |  |
| ingress.hosts[0].paths[0].pathType | string | `"ImplementationSpecific"` |  |
| ingress.tls | list | `[]` |  |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| podAnnotations | object | `{}` |  |
| podLabels | object | `{}` |  |
| podSecurityContext | object | `{}` |  |
| replicaCount | int | `1` |  |
| resources | object | `{}` |  |
| securityContext | object | `{}` |  |
| service.port | int | `80` |  |
| service.type | string | `"ClusterIP"` |  |
| serviceAccount.annotations | object | `{}` |  |
| serviceAccount.automount | bool | `true` |  |
| serviceAccount.create | bool | `true` |  |
| serviceAccount.name | string   || `""` |  |
| tolerations | list | `[]` |  |
| volumeMounts | list | `[]` |  |
| volumes | list | `[]` |  |
