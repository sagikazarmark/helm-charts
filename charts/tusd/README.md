# tusd

![version: 0.0.1](https://img.shields.io/badge/version-0.0.1-informational?style=flat-square) ![type: application](https://img.shields.io/badge/type-application-informational?style=flat-square) ![app version: 1.6.0](https://img.shields.io/badge/app%20version-1.6.0-informational?style=flat-square) ![kube version: >=1.16.0-0](https://img.shields.io/badge/kube%20version->=1.16.0--0-informational?style=flat-square) [![artifact hub](https://img.shields.io/badge/artifact%20hub-tusd-informational?style=flat-square)](https://artifacthub.io/packages/helm/sagikazarmark/tusd)

Reference server implementation in Go of tus: the open protocol for resumable file uploads.

**Homepage:** <https://tus.io>

## TL;DR;

```bash
helm repo add skm https://charts.sagikazarmark.dev
helm install --generate-name --wait skm/tusd
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| replicaCount | int | `1` | Number of Pods to launch. |
| image.repository | string | `"tusproject/tusd"` | Repository to pull the container image from. |
| image.pullPolicy | string | `"IfNotPresent"` | Image [pull policy](https://kubernetes.io/docs/concepts/containers/images/#updating-images) |
| image.tag | string | `""` | Overrides the image tag (default is the chart appVersion). |
| imagePullSecrets | list | `[]` | Image [pull secrets](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-pod-that-uses-your-secret) |
| nameOverride | string | `""` | Provide a name in place of the chart name for `app:` labels. |
| fullnameOverride | string | `""` | Provide a name to substitute for the full names of resources. |
| volumes | list | `[]` | Additional storage [volumes](https://kubernetes.io/docs/concepts/storage/volumes/) of a Pod. See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workloads-resources/pod-v1/#volumes) for details. |
| volumeMounts | list | `[]` | Additional [volume mounts](https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/) of a container. See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workloads-resources/container/#volumes) for details. |
| envFrom | list | `[]` | Configure a [Secret](https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables) or a [ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#configure-all-key-value-pairs-in-a-configmap-as-container-environment-variables) as environment variable sources for a Pod. See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workloads-resources/container/#environment-variables) for details. |
| env | object | `{}` | Pass environment variables directly to a Pod. See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workloads-resources/container/#environment-variables) for details. |
| serviceAccount.create | bool | `true` | Whether a service account should be created. |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account. |
| serviceAccount.name | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template. |
| podAnnotations | object | `{}` | Custom annotations for a Pod. |
| podSecurityContext | object | `{}` | Pod [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod). See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workloads-resources/pod-v1/#security-context) for details. |
| securityContext | object | `{}` | Container [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-container). See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workloads-resources/container/#security-context) for details. |
| containerArgs | list | `[]` | List of container arguments. **Note:** This is a temporary solution until there is a better way to configure the application. See [sagikazarmark/helm-charts#49](https://github.com/sagikazarmark/helm-charts/issues/49) for details. |
| service.annotations | object | `{}` | Custom annotations for the Service. |
| service.type | string | `"ClusterIP"` | Kubernetes [service type](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types). |
| service.loadBalancerIP | string | `nil` | Only applies when service tyep is LoadBalancer. Load balancer will get created with the IP specified in this field. |
| service.loadBalancerSourceRanges | list | `[]` | (list) If specified (and supported by the cloud provider), traffic through the load balancer will be restricted to the specified client IPs. Valid values are IP CIDR blocks. |
| service.port | int | `80` | Service port |
| service.nodePort | int | `nil` | Service node port (when applicable) |
| service.externalTrafficPolicy | string | `nil` | Route external traffic to node-local or cluster-wide endoints. Useful for [preserving the client source IP](https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/#preserving-the-client-source-ip). |
| ingress.enabled | bool | `false` | Enable ingress. |
| ingress.annotations | object | `{}` | Annotations to add to the ingress. |
| ingress.hosts | list | See [values.yaml](values.yaml). | Ingress host configuration. |
| ingress.tls | list | See [values.yaml](values.yaml). | Ingress TLS configuration. |
| resources | object | No requests or limits. | Container resource [requests and limits](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/). See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workloads-resources/container/#resources) for details. |
| autoscaling | object | Disabled by default. | Autoscaling configuration (see [values.yaml](values.yaml) for details). |
| nodeSelector | object | `{}` | [Node selector](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector) configuration. |
| tolerations | list | `[]` | [Tolerations](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/) for node taints. See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workloads-resources/pod-v1/#scheduling) for details. |
| affinity | object | `{}` | [Affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity) configuration. See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workloads-resources/pod-v1/#scheduling) for details. |
