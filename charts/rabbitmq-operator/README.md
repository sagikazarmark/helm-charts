# rabbitmq-operator

![version: 0.0.3](https://img.shields.io/badge/version-0.0.3-informational?style=flat-square) ![type: application](https://img.shields.io/badge/type-application-informational?style=flat-square) ![app version: 1.8.3](https://img.shields.io/badge/app%20version-1.8.3-informational?style=flat-square) ![kube version: >=1.18.0-0](https://img.shields.io/badge/kube%20version->=1.18.0--0-informational?style=flat-square) [![artifact hub](https://img.shields.io/badge/artifact%20hub-rabbitmq--operator-informational?style=flat-square)](https://artifacthub.io/packages/helm/sagikazarmark/rabbitmq-operator)

Kubernetes operator to deploy and manage RabbitMQ clusters.

**Homepage:** <https://www.rabbitmq.com/kubernetes/operator/operator-overview.html>

## TL;DR;

```bash
helm repo add skm https://charts.sagikazarmark.dev
helm install --generate-name --wait skm/rabbitmq-operator
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| replicaCount | int | `1` | Number of replicas (pods) to launch. |
| image.repository | string | `"rabbitmqoperator/cluster-operator"` | Name of the image repository to pull the container image from. |
| image.pullPolicy | string | `"IfNotPresent"` | [Image pull policy](https://kubernetes.io/docs/concepts/containers/images/#updating-images) for updating already existing images on a node. |
| image.tag | string | `""` | Image tag override for the default value (chart appVersion). |
| imagePullSecrets | list | `[]` | Reference to one or more secrets to be used when [pulling images](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-pod-that-uses-your-secret) (from private registries). |
| nameOverride | string | `""` | A name in place of the chart name for `app:` labels. |
| fullnameOverride | string | `""` | A name to substitute for the full names of resources. |
| serviceAccount.create | bool | `true` | Enable service account creation. |
| serviceAccount.annotations | object | `{}` | Annotations to be added to the service account. |
| serviceAccount.name | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template. |
| rbac.create | bool | `true` | Enable the creation of RBAC resources. If disabled, the operator (ie. the person installing the chart) is responsible for creating the necessary resources based on the templates. |
| podAnnotations | object | `{}` | Annotations to be added to pods. |
| priorityClassName | string | `""` | Specify a priority class name to set [pod priority](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/#pod-priority). |
| podSecurityContext | object | `{}` | Pod [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod). See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/#security-context) for details. |
| securityContext | object | `{}` | Container [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-container). See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/#security-context-1) for details. |
| podMonitor.enabled | bool | `false` | Enable Prometheus PodMonitor to monitor the operator. |
| podMonitor.interval | string | `"30s"` | Interval at which metrics should be scraped. |
| resources | object | No requests or limits. | Container resource [requests and limits](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/). See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/#resources) for details. |
| nodeSelector | object | `{}` | [Node selector](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector) configuration. |
| tolerations | list | `[]` | [Tolerations](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/) for node taints. See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/#scheduling) for details. |
| affinity | object | `{}` | [Affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity) configuration. See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/#scheduling) for details. |
| rabbitmq.serviceMonitor.enabled | bool | `false` | Enable Prometheus ServiceMonitor to monitor RabbitMQ clusters created by the operator. |
| rabbitmq.serviceMonitor.interval | string | `"30s"` | Interval at which metrics should be scraped. |

## Development

### Updating the operator

Always start by getting the latest manifest for the operator:

```bash
VERSION="1.7.0"

mkdir tmp

curl -L "https://github.com/rabbitmq/cluster-operator/releases/download/v${VERSION}/cluster-operator.yml" > tmp/cluster-operator.yaml
```

Download the k8split tool:

```bash
go get github.com/brendanjryan/k8split
```

Split up the resources:

```bash
k8split -o tmp tmp/cluster-operator.yaml
```

Update the templates based on the created resources.

Finally, clean up the resources:

```bash
rm -rf tmp
```

## Attribution

This chart is created from the "remains" of the official chart removed in [#296](https://github.com/rabbitmq/cluster-operator/pull/296).
