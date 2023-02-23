# ziti-host

![Version: 0.3.5](https://img.shields.io/badge/Version-0.3.5-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 0.20.20](https://img.shields.io/badge/AppVersion-0.20.20-informational?style=flat-square)

Host OpenZiti services with a tunneler pod

## Requirements

Kubernetes: `>= 1.20.0`

## Overview

You may use this chart to publish cluster services to your Ziti network. For example, if you create a Ziti service with a server address of `tcp:kubernetes.default.svc:443` and write a Bind Service Policy assigning the service to the Ziti identity used with this chart, then your Ziti network's authorized clients will be able access this cluster's apiserver. You could do the same thing for any cluster service's domain name.

## How this Chart Works

This chart deploys a pod running `ziti-edge-tunnel`, [the OpenZiti Linux tunneler](https://docs.openziti.io/docs/reference/tunnelers/linux/), in service hosting mode. The chart uses container image `docker.io/openziti/ziti-host` which runs `ziti-edge-tunnel run-host`. This puts the Linux tunneler in "hosting" mode which is useful for binding Ziti services without any need for elevated permissions and without any Ziti nameserver or intercepting proxy. You'll be able to publish any server that is known by an IP address or domain name that is reachable from the pod deployed by this chart.

## Installation

After adding the charts repo to Helm then you may install the chart. You must supply a Ziti identity JSON file when you install the chart.

```bash
helm install ziti-release03 openziti/ziti-host --set-file zitiIdentity=/tmp/k8s-tunneler-03.json
```

### Installation using a existing / pre-created secret

Alternatively when you want to use a existing / pre-created secret (i.e. you have sealed-secrets enabled in your setup), you could refer to an existing secret with the ziti identity to use.

This sample shows you how to create the secret:

```bash
kubectl create secret generic k8s-tunneler-03-identity --from-file=persisted-identity=k8s-tunneler-03.json
```

When you deploy the helm chart refer to the existing secret:

```bash
helm install ziti-release03 openziti/ziti-host --set secret.existingSecretName=k8s-tunneler-03-identity
```

When you don't want to use the default key name `persisted-identity` you can define your own name by adding `--set secret.keyName=myKeyName`.

## Values Reference

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| dnsPolicy | string | `"ClusterFirstWithHostNet"` |  |
| fullnameOverride | string | `""` |  |
| hostNetwork | bool | `false` |  |
| image.args | list | `[]` |  |
| image.pullPolicy | string | `"Always"` |  |
| image.repository | string | `"openziti/ziti-host"` |  |
| imagePullSecrets | list | `[]` |  |
| ingress.enabled | bool | `false` |  |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| persistence.accessMode | string | `"ReadWriteOnce"` |  |
| persistence.enabled | bool | `true` |  |
| persistence.size | string | `"100Mi"` |  |
| podAnnotations | object | `{}` |  |
| podSecurityContext | object | `{}` |  |
| ports | list | `[]` |  |
| replicas | int | `1` |  |
| resources | object | `{}` |  |
| secret | object | `{}` |  |
| securityContext | object | `{}` |  |
| serviceAccount.annotations | object | `{}` |  |
| serviceAccount.create | bool | `true` |  |
| serviceAccount.name | string | `""` |  |
| tolerations | list | `[]` |  |

```bash
helm upgrade {release} {source dir}
```

<!-- generated with helm-docs -->