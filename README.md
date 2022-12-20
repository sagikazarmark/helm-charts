# Helm Charts

![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/sagikazarmark/helm-charts/release.yaml?branch=master&style=flat-square)
[![Artifact HUB](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/sagikazarmark)](https://artifacthub.io/packages/search?repo=sagikazarmark)
![Kubernetes Version](https://img.shields.io/badge/k8s%20version-%3E=1.19-3f68d7.svg?style=flat-square)

Various Helm [charts](https://helm.sh/docs/topics/charts/) for my own and other projects.


## Usage

[Helm](https://helm.sh) must be installed to use these charts.
Please refer to the [official documentation](https://helm.sh/docs/intro/install/) to get started.

```bash
helm repo add skm https://charts.sagikazarmark.dev
```

You can then see the charts by running:

```bash
helm search repo skm
```

You can install charts using the following command:

```bash
helm install --generate-name skm/CHART
# OR
helm install --name my-release skm/CHART
```

> **Tip**: List all installed releases using `helm list`.

To uninstall a chart release:

```bash
helm delete my-release
```


## License

The project is licensed under the [MIT License](LICENSE).
