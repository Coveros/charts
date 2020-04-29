# Nexus Repository OSS

[Nexus Repository OSS](https://www.sonatype.com/nexus-repository-oss) is a free open source repository manager. It supports a wide 
range of package formats and it's used by hundreds of tech companies.

## Introduction

This chart bootstraps a Nexus Repository OSS deployment on a cluster using Helm.

It was originally forked from https://github.com/Oteemo/charts/tree/master/charts/sonatype-nexus

## Prerequisites

- Kubernetes 1.17+
- [Fulfill Nexus kubernetes requirements](https://github.com/travelaudience/kubernetes-nexus#pre-requisites)

## Testing the Chart
To test the chart:
```bash
$ helm install --dry-run --debug ./
```
To test the chart with your own values:
```bash
$ helm install --dry-run --debug -f my_values.yaml ./
```

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install  my-release coveros.github.io/charts/nexus-repository
```

The above command deploys Nexus on the Kubernetes cluster in the default configuration. The [configuration](#configuration) 
section lists the parameters that can be configured during installation.

The default login is admin/admin123

## Uninstalling the Chart

To uninstall/delete the deployment:

```bash
$ helm list
NAME           	REVISION	UPDATED                 	STATUS  	CHART      	NAMESPACE
plinking-gopher	1       	Fri Sep  1 13:19:50 2017	DEPLOYED	nexus-repository-0.1.0	default
$ helm delete plinking-gopher
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Official Nexus image vs TravelAudience

There are known issues with backups on the official image. If you want to swap in the official image, just override the 
values when installing the chart. Please note that backups will not work as expected with the official image.

* [https://issues.sonatype.org/browse/NEXUS-23442](https://issues.sonatype.org/browse/NEXUS-23442)
* [https://github.com/travelaudience/docker-nexus](https://github.com/travelaudience/docker-nexus)

## Configuration

The following table lists the configurable parameters of the Nexus chart and their default values.

| Parameter                                   | Description                         | Default                                 |
| ------------------------------------------  | ----------------------------------  | ----------------------------------------|
| `config.data`                               | Configmap data                      | `{}`                                    |
| `config.enabled`                            | Enable configmap                    | `false`                                 |
| `config.mountPath`                          | Path to mount the config            | `/sonatype-nexus-conf`                  |
| `deployment.additionalContainers`           | Add additional Containers           | Not Set                                 |
| `deployment.additionalVolumes`              | Add additional Volumes              | Not Set                                 |
| `deployment.additionalVolumeMounts`         | Add additional Volume mounts        | Not Set                                 |
| `deployment.annotations`                    | Annotations to enhance deployment configuration  | `{}`                       |
| `deployment.labels`                         | Custom deployment labels            | `{}`                                    |
| `deployment.initContainers`                 | Init containers to run before main containers  | Not Set                      |
| `deployment.postStart.command`              | Command to run after starting the nexus container  | Not Set                  |
| `deploymentStrategy`                        | Deployment Strategy                 | `{}`                                    |
| `ingress.annotations`                       | Annotations to enhance ingress configuration  | `{}`                          |
| `ingress.enabled`                           | Create an ingress for Nexus         | `false`                                 |
| `ingress.path`                              | Path for ingress rules.             | `/`                                     |
| `ingress.labels`                            | Ingress Labels                      | `{}`                                    |
| `ingress.tls.enabled`                       | Enable TLS                          | `true`                                  |
| `ingress.tls.secretName`                    | Name of the secret storing TLS cert, `false` to use the Ingress' default certificate | `nexus-tls` |
| `nexus.imageRepository`                     | Nexus image                         | `quay.io/travelaudience/docker-nexus`   |
| `nexus.imageTag`                            | Version of Nexus                    | `3.22.0-02`                             |
| `nexus.imagePullPolicy`                     | Nexus image pull policy             | `IfNotPresent`                          |
| `nexus.imagePullSecretName`                 | Secret to download Nexus image from private registry      | Not Set           |
| `nexus.priorityClassName`                   | Schedule pods on priority           | Not Set                                 |
| `nexus.env`                                 | Nexus environment variables         | `[{install4jAddVmParams: -Xms1200M -Xmx1200M -XX:MaxDirectMemorySize=2G -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap}]` |
| `nexus.nodeSelector`                        | Node labels for pod assignment      | `{}`                                    |
| `nexus.resources`                           | Nexus resource requests and limits  | `{}`                                    |
| `nexus.securityContextEnabled`              | Enable security context             | `true`                                  |
| `nexus.securityContext`                     | Security Context (for enabling official image use `fsGroup: 200`) | `true`    |
| `nexus.podAnnotations`                      | Pod Annotations                     | `{}`                                    |
| `nexus.livenessProbe.initialDelaySeconds`   | LivenessProbe initial delay         | `30`                                    |
| `nexus.livenessProbe.periodSeconds`         | Seconds between polls               | `30`                                    |
| `nexus.livenessProbe.failureThreshold`      | Number of attempts before failure   | `6`                                     |
| `nexus.livenessProbe.timeoutSeconds`        | Time in seconds after liveness probe times out    | Not Set                   |
| `nexus.livenessProbe.path`                  | Path for LivenessProbe              | `/`                                     |
| `nexus.readinessProbe.initialDelaySeconds`  | ReadinessProbe initial delay        | `30`                                    |
| `nexus.readinessProbe.periodSeconds`        | Seconds between polls               | `30`                                    |
| `nexus.readinessProbe.failureThreshold`     | Number of attempts before failure   | `6`                                     |
| `nexus.readinessProbe.timeoutSeconds`       | Time in seconds after readiness probe times out    | Not Set                  |
| `nexus.readinessProbe.path`                 | Path for ReadinessProbe             | `/`                                     |
| `nexus.hostAliases`                         | Aliases for IPs in /etc/hosts       | `[]`                                    |
| `persistence.enabled`                       | Create a volume for storage         | `true`                                  |
| `persistence.accessMode`                    | ReadWriteOnce or ReadOnly           | `ReadWriteOnce`                         |
| `persistence.storageClass`                  | Storage class of Nexus PVC          | Not Set                                 |
| `persistence.storageSize`                   | Size of Nexus data volume           | `8Gi`                                   |
| `persistence.annotations`                   | Persistent Volume annotations       | `{}`                                    |
| `persistence.existingClaim`                 | Existing PVC name                   | Not Set                                 |
| `replicaCount`                              | Number of Nexus service replicas    | `1`                                     |
| `statefulset.enabled`                       | Use statefulset instead of deployment | `false`                               |
| `secret.enabled`                            | Enable secret                       | `false`                                 |
| `secret.mountPath`                          | Path to mount the secret            | `/etc/secret-volume`                    |
| `secret.readOnly`                           | Secret readonly state               | `true`                                  |
| `secret.data`                               | Secret data                         | `{}`                                    |
| `service.type`                              | Service type                        | `ClusterIP`                             |
| `service.annotations`                       | Service annotations                 | `{}`                                    |
| `service.labels`                            | Service labels                      | `{}`                                    |
| `service.docker.port`                       | Docker service port                 | `5003`                                  |
| `service.docker.targetPort`                 | Docker target port                  | `5003`                                  |
| `service.docker.nodePort`                   | Docker node port                    | Not Set                                 |
| `service.http.port`                         | Nexus service port                  | `8081`                                  |
| `service.http.targetPort`                   | Nexus target port                   | `8081`                                  |
| `service.http.nodePort`                     | Nexus node port                     | Not Set                                 |
| `service.loadBalancerSourceRanges`          | Service LoadBalancer source IP whitelist | Not Set                            |
| `serviceAccount.create`                     | Automatically create a service account | `true`                               |
| `serviceAccount.name`                       | Service account to use 				| Not Set								  |
| `serviceAccount.annotations`                | Service account annotations			| Not Set	   							  |
| `tolerations`                               | tolerations list                    | `[]`                                    |
| `additionalConfigMaps`                      | List of ConfigMap data containing Name, Data and Labels | Not Set             |

```bash
$ helm install my-release --set persistence.enabled=false coveros/nexus-repository
```
The above example turns off the persistence. Data will not be kept between restarts or deployments

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install my-release -f my-values.yaml coveros/nexus-repository
```

### Persistence

By default a PersistentVolumeClaim is created and mounted into the `/nexus-data` directory. In order to disable this functionality
you can change the `values.yaml` to disable persistence which will use an `emptyDir` instead.

> *"An emptyDir volume is first created when a Pod is assigned to a Node, and exists as long as that Pod is running on 
>that node. When a Pod is removed from a node for any reason, the data in the emptyDir is deleted forever."*

You must enable StatefulSet (`statefulset.enabled=true`) for true data persistence. If using Deployment approach, you 
can not recover data after restart or delete of helm chart. Statefulset will make sure that it picks up the same old 
volume which was used by the previous life of the nexus pod, helping you recover your data. When enabling statefulset, 
its required to enable the persistence.

## After Installing the Chart
After installing the chart a couple of actions need still to be done in order to use nexus. Please follow the instructions below.

### Nexus Configuration
The following steps need to be executed in order to use Nexus:

- [Configure Nexus](https://github.com/travelaudience/kubernetes-nexus/blob/master/docs/admin/configuring-nexus.md)

### Nexus Usage
To see how to use Nexus with different tools like Docker, Maven, Python, and so on please check:

- [Nexus Usage](https://github.com/travelaudience/kubernetes-nexus#usage)

### Disaster Recovery
In a disaster recovery scenario, the latest backup made by the nexus-backup container should be restored. In order to 
achieve this please follow the procedure described below:
- [Restore Backups](https://github.com/travelaudience/kubernetes-nexus#restore)
