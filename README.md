# Deploy using Helm Charts

Other relevant repositories of the [Lock-Unlock Project](https://labs.kadaster.nl/cases/lockunlock) are:

- [Lock-Unlock Onthologies](https://github.com/kadaster-labs/lock-unlock-onthologies)
  - Authorization Onthology (in research)
  - Logging Onthology (in research)
- [Lock-Unlock Testdata](https://github.com/kadaster-labs/lock-unlock-testdata)
- [Secured SPARQL Endpoint Sub Graph](https://github.com/kadaster-labs/secured-sparql-endpoint) (based on Apache Jena & SpringBoot)
- [Secured SPARQL Endpoint Rewrite (SPARQL Query)](https://github.com/kadaster-labs/secured-sparql-endpoint-rewrite) (based on Fuseki)
- (this repo) [Lock-Unlock Helm Charts](https://github.com/kadaster-labs/lock-unlock-helm-charts)

## Test on a local Docker Desktop Kubernetes environment:
For IngressController (to access the Fuseki endpoint in the browser), install something like [ingress-nginx](https://kubernetes.github.io/ingress-nginx/).
```bash
helm upgrade --install ingress-nginx ingress-nginx  \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```

The next step assumes you've built the lock-unlock-rewrite and lock-unlock-dataloader images in their corresponding repos, as a reminder:
```bash
# cd secured-sparql-endpoint-rewrite
docker build --build-arg JENA_VERSION=4.10.0 --build-arg LOCK_UNLOCK_VERSION=0.1.0 -t lock-unlock/rewrite:0.1.0 -f docker/Dockerfile .

# cd lock-unlock-testdata
docker build -t lock-unlock/dataloader:0.1.0 ./lock-unlock-dataloader
```

Then, the example deployment for a BRK deployment can be done using the following command:
```bash
helm upgrade --install fuseki charts/lock-unlock-rewrite \
  --values examples/lock-unlock-rewrite/values.localhost.yaml \
  --namespace lock-unlock --create-namespace
```

You should now be able to access the Fuseki UI at http://fuseki.127.0.0.1.nip.io/

### Development
The generated Kubernetes files can be checked by doing something like the following, which might be useful for debugging:
```bash
helm template charts/lock-unlock-rewrite --values values.localhost.yaml --debug > foo.yaml
```

## Deploying multiple dataset
You might do something like:
```bash
DATASET=anbi

helm upgrade --install fuseki-$DATASET charts/lock-unlock-rewrite \
  --namespace lock-unlock-$DATASET --create-namespace \
  --values examples/lock-unlock-rewrite/values.localhost.yaml \
  --set ingress.hosts\[0\].host=$DATASET.127.0.0.1.nip.io \
  --set dataloader.dataset.file_url="https://raw.githubusercontent.com/kadaster-labs/lock-unlock-testdata/main/testdata-$DATASET/lock-unlock-$DATASET.ttl.gz" \
  --set dataloader.dataset.endpoint="/$DATASET"
```

You can then access http://$DATASET.127.0.0.1.nip.io/

Acceptable values DATASET currently are: `anbi`, `brk`, `brp` and `nhr`.

## License

Licensed under [EUPL-1.2](LICENSE.md)
