# Testing lock-unlock-rewrite locally
## Test on a local Docker Desktop Kubernetes environment:
For IngressController (to access the Fuseki endpoint in the browser), install something like [ingress-nginx](https://kubernetes.github.io/ingress-nginx/).
```console
helm upgrade --install ingress-nginx ingress-nginx  \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```

The next step assumes you've built the [lock-unlock-rewrite](https://github.com/kadaster-labs/secured-sparql-endpoint-rewrite) and [lock-unlock-dataloader](https://github.com/kadaster-labs/lock-unlock-testdata/tree/main/lock-unlock-dataloader) images in their corresponding repos, as a reminder:
```console
# cd secured-sparql-endpoint-rewrite
docker build --build-arg JENA_VERSION=4.10.0 --build-arg LOCK_UNLOCK_VERSION=0.1.4 -t lock-unlock/rewrite:0.1.4 -f docker/Dockerfile .

# cd lock-unlock-testdata
docker build -t lock-unlock/dataloader:0.1.1 ./lock-unlock-dataloader
```

Then, the example deployment for a BRK deployment can be done using the following command:
```console
helm upgrade --install fuseki charts/lock-unlock-rewrite \
  --values examples/lock-unlock-rewrite/values.localhost.yaml \
  --namespace lock-unlock --create-namespace
```

You should now be able to access the Fuseki UI at http://fuseki.127.0.0.1.nip.io/

## Deploying multiple dataset
You might do something like:
```console
DATASET=anbi

helm upgrade --install fuseki-$DATASET charts/lock-unlock-rewrite \
  --namespace lock-unlock-$DATASET --create-namespace \
  --values examples/lock-unlock-rewrite/values.localhost.yaml \
  --set ingress.hosts\[0\].host=$DATASET.127.0.0.1.nip.io \
  --set dataloader.dataset.file_url="https://raw.githubusercontent.com/kadaster-labs/lock-unlock-testdata/main/testdata-$DATASET/data.zip" \
  --set dataloader.dataset.endpoint="/$DATASET"
```

You can then access http://$DATASET.127.0.0.1.nip.io/

Acceptable values DATASET currently are: `anbi`, `brk`, `brp` and `nhr`.

## Development
The generated Kubernetes files can be inspected by doing something like the following, which might be useful for debugging:
```console
helm template charts/lock-unlock-rewrite --values values.localhost.yaml --debug > foo.yaml
```
