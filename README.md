# Container Security Operator
The Container Security Operator provides a "no-configuration" pod scanning interface.
Once installed, the Container Security Operator provides:
- Security information about images running in cluster
- Metrics via [Prometheus](https://prometheus.io)

## ImageManifestVuln
The security information of scanned images are stored in `ImageManifestVulns` on an image manifest basis, and are named by the image's manifest digest.

### Spec
The spec provides information about the features and its associated vulnarabilities.
The spec should be immutable relative to the cluster. When a new vulnerability is added to a feature, the operator will update the spec after the resync interval.

### Status
The status provides information about the affected Pods/Containers. As pod are added or removed
from the cluster, their references are added to the `affectedPods` field of the status block.
The status also provide various statistics about the manifest. e.g lastUpdate, highestSeverity, ...

## Example config
```yaml
securitylabeller:
  prometheusAddr: "0.0.0.0:8081"
  interval: 15m
  wellknownEndpoint: ".well-known/app-capabilities"
  labelPrefix: secscan
  namespaces:
    - default
    - test
```

The same options can be configured from the command line:
```
./container-security-operator -promAddr ":8081" -resyncInterval "15m" -wellknownEndpoint ".well-known/app-capabilities" -labelPrefix "secscan" -namespace default -namespace test
```

## Development Environment

Running the labeller locally requires a valid kubeconfig.
If the kubeconfig flag is omitted, an in-cluster config is assumed.

Install the ImageManifestVuln CRD
```
make installcrds
```

Running locally (using `~/.kube/config` and `example-config.yaml`):
```
make run
```

To regenerate the CRD code:
```
# deepcopy
make deepcopy
# openapi
make openapi
# clientset
make clientset
# listers
make listers
# informers
make informers
# generate all
codegen
# generate all in a container
codegen-container
```

## Deployment
TODO
