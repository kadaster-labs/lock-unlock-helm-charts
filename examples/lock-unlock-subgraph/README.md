# Testing lock-unlock-subgraph locally

## Test on a local Docker Desktop Kubernetes environment:

For IngressController (to access the Subgraph endpoint in the browser), install something like [ingress-nginx](https://kubernetes.github.io/ingress-nginx/).
```console
helm upgrade --install ingress-nginx ingress-nginx  \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```

The next step assumes you've built the [lock-unlock-subgraph](https://github.com/kadaster-labs/secured-sparql-endpoint-subgraph) image in its corresponding repo, as a reminder:

```console
# cd secured-sparql-endpoint-subgraph
docker build --build-arg LOCK_UNLOCK_VERSION=0.0.1 -t lock-unlock/subgraph:0.0.1 -f docker/Dockerfile .
```

Then, the example deployment for a BRK deployment can be done using the following command:

```console
helm upgrade --install subgraph charts/lock-unlock-subgraph \
  --values examples/lock-unlock-subgraph/values.localhost.yaml \
  --namespace lock-unlock --create-namespace
```

You should now be able to access the Subgraph UI at http://subgraph.127.0.0.1.nip.io/

## Development

The generated Kubernetes files can be inspected by doing something like the following, which might be useful for debugging:

```console
helm template charts/lock-unlock-subgraph --values examples/lock-unlock-subgraph/values.localhost.yaml --debug > foo.yaml
```
