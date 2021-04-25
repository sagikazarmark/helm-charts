# kube-secrets-init

![version: 0.7.0](https://img.shields.io/badge/version-0.7.0-informational?style=flat-square) ![type: application](https://img.shields.io/badge/type-application-informational?style=flat-square) ![app version: 0.4.0](https://img.shields.io/badge/app%20version-0.4.0-informational?style=flat-square) ![kube version: >=1.16.0-0](https://img.shields.io/badge/kube%20version->=1.16.0--0-informational?style=flat-square) [![artifact hub](https://img.shields.io/badge/artifact%20hub-kube--secrets--init-informational?style=flat-square)](https://artifacthub.io/packages/helm/sagikazarmark/kube-secrets-init)

kube-secrets-init is a Kubernetes mutating admission webhook, that mutates any Pod that is using specially prefixed environment variables, directly or from Kubernetes as Secret or ConfigMap.

**Homepage:** <https://github.com/doitintl/kube-secrets-init>

## TL;DR;

```bash
helm repo add skm https://charts.sagikazarmark.dev
helm install --generate-name --wait skm/kube-secrets-init
```

## Configuration

### Certificate

Webhooks require CA and TLS certificates to secure communication between the API server and the webhook.
The chart offers the following options for certificate provisioning:

#### Generate (default)

The default option is to let Helm generate the CA and TLS certificates during installation.

This will renew the certificates on each deployment.

```yaml
certificate:
  generate: true
```

#### Using cert-manager

If have [cert-manager](https://cert-manager.io/) installed in your cluster, you can use it to automatically provision and inject certificates.

```yaml
certificate:
  useCertManager: true
  generate: false
```

### Init container image

By default the webhook uses the latest init container image (`doitintl/secrets-init:latest`).
It's a good practice though to use a concrete version, because a faulty update can easily bring down your workloads (like in [this](https://github.com/doitintl/kube-secrets-init/issues/21) case):

```yaml
initContainer:
  image:
    tag: 0.2.12
```

Make sure that the webhook version if compatible with the init container version.

### Availability and disruptions

Webook disruptions can easily render clusters inoperational by preventing launching new pods leading to unplanned outages.
Kubernetes offers different features to keep that from happening:

- Fine-tune resource requests and limits to help the scheduler placing pods on nodes with enough resources.
- Run multiple replicas for higher availability.
- Spread pods across failure-domains (regions, zones, nodes) for even higher availability.
- Define a [pod distruption budget](https://kubernetes.io/docs/tasks/run-application/configure-pdb/) for voluntary disruptions.

Here is an example configuration for higher availability,
but make sure to customize it to your needs before using it in production:

```yaml
replicas: 3

resources:
  limits:
    cpu: 100m
    memory: 128Mi
  requests:
    cpu: 50m
    memory: 32Mi

affinity:
  podAntiAffinity:
    preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        podAffinityTerm:
          topologyKey: kubernetes.io/hostname
          labelSelector:
            matchLabels:
              app.kubernetes.io/name: kube-secrets-init
              app.kubernetes.io/instance: kube-secrets-init

podDisruptionBudget:
  enabled: true
  minAvailable: 2
```

You can further reduce the blast-radius of involuntary disruptions by configuring `namespaceSelector` and/or `objectSelector`
to limit the webhooks to the required namespaces/workloads.

Read more about dealing with disruptions in the [Kubernetes documentation](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/).

## About GKE Private Clusters

When Google configure the control plane for private clusters, they automatically configure VPC peering between your Kubernetes cluster’s network in a separate Google managed project.

The auto-generated rules **only** open ports 10250 and 443 between masters and nodes. This means that to use the webhook component with a GKE private cluster, you must configure an additional firewall rule to allow your masters CIDR to access your webhook pod using the port 8443.

You can read more information on how to add firewall rules for the GKE control plane nodes in the [GKE docs](https://cloud.google.com/kubernetes-engine/docs/how-to/private-clusters#add_firewall_rules).

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| replicaCount | int | `1` | Number of replicas (pods) to launch. |
| image.repository | string | `"ghcr.io/doitintl/kube-secrets-init"` | Name of the image repository to pull the container image from. |
| image.pullPolicy | string | `"IfNotPresent"` | [Image pull policy](https://kubernetes.io/docs/concepts/containers/images/#updating-images) for updating already existing images on a node. |
| image.tag | string | `""` | Image tag override for the default value (chart appVersion). |
| imagePullSecrets | list | `[]` | Reference to one or more secrets to be used when [pulling images](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/#create-a-pod-that-uses-your-secret) (from private registries). |
| nameOverride | string | `""` | A name in place of the chart name for `app:` labels. |
| fullnameOverride | string | `""` | A name to substitute for the full names of resources. |
| provider | string | `""` | One of the supported secret providers:   - `google` (Google Cloud Secrets Manager)   - `aws` (AWS Secrets Manager and SSM Parameter Store) |
| defaultImagePullSecret.name | string | `""` | Fallback secret name to use when no image pull secret is found. |
| defaultImagePullSecret.namespace | string | `""` | Namespace of the fallback secret name. |
| initContainer.image.repository | string | `ghcr.io/doitintl/secrets-init` (from CLI defaults) | Name of the image repository to pull the init container image from. |
| initContainer.image.pullPolicy | string | `IfNotPresent` (from CLI defaults) | [Image pull policy](https://kubernetes.io/docs/concepts/containers/images/#updating-images) for updating already existing images on a node. |
| initContainer.image.tag | string | `latest` (from CLI defaults) | Image tag for the init container. **Note:** it is **strongly** recommended to change the image tag to avoid issues like [this](https://github.com/doitintl/kube-secrets-init/issues/21). |
| certificate.useCertManager | bool | `false` | Use jetstack/cert-manager for creating the necessary certificates. This is usually preferred as cert-manager automatically renews certificates. Mutually exclusive with `generate`. |
| certificate.generate | bool | `true` | Generate the necessary certificates during chart install. Mutually exclusive with `useCertManager`. |
| certificate.secretName | string | `""` | The name of the secret to use. If not set and useCertManager or generate is true, a name is generated using the fullname template. |
| serviceAccount.create | bool | `true` | Enable service account creation. |
| serviceAccount.annotations | object | `{}` | Annotations to be added to the service account. |
| serviceAccount.name | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template. |
| rbac.create | bool | `true` | Enable the creation of RBAC resources. If disabled, the operator (ie. the person installing the chart) is responsible for creating the necessary resources based on the templates. |
| podAnnotations | object | `{}` | Annotations to be added to pods. |
| podSecurityContext | object | `{}` | Pod [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod). See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/#security-context) for details. |
| securityContext | object | `{}` | Container [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-container). See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/#security-context-1) for details. |
| serviceMonitor.enabled | bool | `false` | Enable Prometheus ServiceMonitor. |
| serviceMonitor.interval | string | `"30s"` | Interval at which metrics should be scraped. |
| resources | object | No requests or limits. | Container resource [requests and limits](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/). See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/#resources) for details. |
| autoscaling | object | Disabled by default. | Autoscaling configuration (see [values.yaml](values.yaml) for details). |
| nodeSelector | object | `{}` | [Node selector](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector) configuration. |
| tolerations | list | `[]` | [Tolerations](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/) for node taints. See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/#scheduling) for details. |
| affinity | object | `{}` | [Affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity) configuration. See the [API reference](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/#scheduling) for details. |
| namespaceSelector | object | `kube-system` namespace is excluded. | [Namespace selector](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/#matching-requests-namespaceselector) for the mutating webhook configuration. |
| objectSelector | object | Exclude objects labeled with `kube-init-secrets.doit-intl.com/mutate: skip`. | [Object selector](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/#matching-requests-objectselector) for the mutating webhook configuration. |
| podDisruptionBudget.enabled | bool | `false` | Enable a [pod distruption budget](https://kubernetes.io/docs/tasks/run-application/configure-pdb/) to help dealing with [disruptions](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/). It is **highly recommended** for webhooks as disruptions can prevent launching new pods. |
| podDisruptionBudget.minAvailable | int/percentage | `nil` | Number or percentage of pods that must remain available. |
| podDisruptionBudget.maxUnavailable | int/percentage | `nil` | Number or percentage of pods that can be unavailable. |

## Attributions

Forked from [banzaicloud-stable/kube-secrets-init](https://github.com/banzaicloud/banzai-charts/tree/cd93b7049885033c36f5e9551bb39fda7361f835/kube-secrets-init).
