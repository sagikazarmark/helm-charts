# SFTPGo

[SFTPGo](https://github.com/drakkan/sftpgo) is a fully featured and highly configurable SFTP server with optional FTP/S and WebDAV support, written in Go. It can serve local filesystem, S3 (compatible) Object Storage, Google Cloud Storage and Azure Blob Storage.


## TL;DR;

```bash
helm repo add skm https://charts.sagikazarmark.dev
helm install --name sftpgo skm/sftpgo
```


## Introduction

This chart bootstraps an [SFTPGo](https://github.com/drakkan/sftpgo) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.


## Prerequisites

- Kubernetes 1.18+
- Helm 3.4+
- SFTPGo 1.2.0+


## Installing the Chart

To install the chart with the release name `my-release`:

```bash
helm install --name my-release skm/sftpgo
```

> **Tip**: List all releases using `helm list`


## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.
