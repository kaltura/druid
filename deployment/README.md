<!--
  ~ Licensed to the Apache Software Foundation (ASF) under one
  ~ or more contributor license agreements.  See the NOTICE file
  ~ distributed with this work for additional information
  ~ regarding copyright ownership.  The ASF licenses this file
  ~ to you under the Apache License, Version 2.0 (the
  ~ "License"); you may not use this file except in compliance
  ~ with the License.  You may obtain a copy of the License at
  ~
  ~   http://www.apache.org/licenses/LICENSE-2.0
  ~
  ~ Unless required by applicable law or agreed to in writing,
  ~ software distributed under the License is distributed on an
  ~ "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
  ~ KIND, either express or implied.  See the License for the
  ~ specific language governing permissions and limitations
  ~ under the License.
  -->

# Apache Druid

[Apache Druid](https://druid.apache.org/) is a high performance real-time analytics database.

## Dependency Update

Before you install the Druid Chart, update the dependencies :
```bash
helm dependency update helm/druid
```

## Install Chart

To install the Druid Chart into your Kubernetes cluster :

```bash
helm install druid helm/druid --namespace dev --create-namespace
```

After installation succeeds, you can get a status of Chart

```bash
helm status druid -n dev 
```

If you want to delete your Chart, use this command:

```bash
helm uninstall druid -n dev
```

### Helm ingresses

The Chart provides ingress configuration to allow customization the installation by adapting
the `values.yaml` depending on your setup.
Please read the comments in the `values.yaml` file for more details on how to configure your reverse
proxy or load balancer.

### Chart Prefix

This Helm automatically prefixes all names using the release name to avoid collisions.

### URL prefix

This chart exposes 6 endpoints:

- Druid Overlord
- Druid Broker
- Druid Coordinator
- Druid Historical
- Druid Middle Manager
- Druid Router

### Druid configuration

Druid configuration can be changed by using environment variables from Docker image.

See the
[Druid Docker entry point](https://github.com/apache/druid/blob/master/distribution/docker/druid.sh)
for more informations

### Middle Manager and Historical Statefulset

Middle Managers and Historicals uses StatefulSet. Persistence is enabled by default.

## Helm chart Configuration

The following table lists the configurable parameters of the Druid chart and their default values.

| Parameter                                | Description                                             | Default                                    |
|------------------------------------------|---------------------------------------------------------|--------------------------------------------|
| `image.repository`                       | container image name                                    | `apache/druid`                             |
| `image.tag`                              | container image tag                                     | `0.19.0`                                   |
| `image.pullPolicy`                       | container pull policy                                   | `IfNotPresent`                             |
| `image.pullSecrets`                      | image pull secrest for private repositoty               | `[]`                                       |
| `configMap.enabled`                       | enable druid configuration as configmap                   | `true`                                     |
| `configVars`                              | druid configuration variables for all components         | ``                                         |
| `gCloudStorage.enabled`                  | look for secret to set google cloud credentials         | `false`                                    |
| `gCloudStorage.secretName`               | secretName to be mounted as google cloud credentials    | `false`                                    |
| `rbac.create`                            | Create roles and roleBindings for service Accounts      | `true`                                     |
| `broker.enabled`                         | enable broker                                           | `true`                                     |
| `broker.name`                            | broker component name                                   | `broker`                                   |
| `broker.replicaCount`                    | broker node replicas (deployment)                       | `1`                                        |
| `broker.port`                            | port of broker component                                | `8082`                                     |
| `broker.serviceAccount.create`           | Create a service account for broker service             | `true`                                     |
| `broker.serviceAccount.name`             | Service account name                                    | Derived from the name of service           |
| `broker.serviceAccount.annotations`      | Annotations applied to created service account          | `{}`                                       |
| `broker.serviceAccount.labels`           | Labels applied to created service account               | `{}`                                       |
| `broker.serviceAccount.automountServiceAccountToken` | Automount API credentials for the Service Account | `true`                               |
| `broker.serviceType`                     | service type for service                                | `ClusterIP`                                |
| `broker.resources`                       | broker node resources requests & limits                 | `{}`                                       |
| `broker.podAnnotations`                  | broker deployment annotations                           | `{}`                                       |
| `broker.nodeSelector`                    | Node labels for broker pod assignment                   | `{}`                                       |
| `broker.tolerations`                     | broker tolerations                                      | `[]`                                       |
| `broker.config`                           | broker private config such as `JAVA_OPTS`                |                                            |
| `broker.affinity`                         | broker affinity policy                                   | `{}`                                       |
| `broker.ingress.enabled`                 | enable ingress                                          | `false`                                    |
| `broker.ingress.hosts`                   | hosts for the broker api                                | `[ "chart-example.local" ]`                |
| `broker.ingress.path`                    | path of the broker api                                  | `/`                                        |
| `broker.ingress.annotations`             | annotations for the broker api ingress                  | `{}`                                       |
| `broker.ingress.tls`                     | TLS configuration for the ingress                        | `[]`                                       |
| `coordinator.enabled`                    | enable coordinator                                      | `true`                                     |
| `coordinator.name`                       | coordinator component name                              | `coordinator`                              |
| `coordinator.replicaCount`               | coordinator node replicas (deployment)                  | `1`                                        |
| `coordinator.port`                       | port of coordinator component                           | `8081`                                     |
| `coordinator.serviceType`                | service type for service                                | `ClusterIP`                                |
| `coordinator.serviceAccount.create`      | Create a service account for coordinator service        | `true`                                     |
| `coordinator.serviceAccount.name`        | Service account name                                    | Derived from the name of service           |
| `coordinator.serviceAccount.annotations` | Annotations applied to created service account          | `{}`                                       |
| `coordinator.serviceAccount.labels`      | Labels applied to created service account               | `{}`                                       |
| `coordinator.serviceAccount.automountServiceAccountToken` | Automount API credentials for the Service Account | `true`                          |
| `coordinator.resources`                  | coordinator node resources requests & limits            | `{}`                                       |
| `coordinator.podAnnotations`             | coordinator Deployment annotations                      | `{}`                                       |
| `coordinator.nodeSelector`               | node labels for coordinator pod assignment              | `{}`                                       |
| `coordinator.tolerations`                | coordinator tolerations                                 | `[]`                                       |
| `coordinator.config`                      | coordinator private config such as `JAVA_OPTS`           |                                            |
| `coordinator.affinity`                    | coordinator affinity policy                              | `{}`                                       |
| `coordinator.ingress.enabled`            | enable ingress                                          | `false`                                    |
| `coordinator.ingress.hosts`              | hosts for the coordinator api                           | `[ "chart-example.local" ]`                |
| `coordinator.ingress.path`               | path of the coordinator api                             | `/`                                        |
| `coordinator.ingress.annotations`        | annotations for the coordinator api ingress             | `{}`                                       |
| `coordinator.ingress.tls`                | TLS configuration for the ingress                        | `[]`                                       |
| `overlord.enabled`                       | enable overlord                                         | `false`                                    |
| `overlord.name`                          | overlord component name                                 | `overlord`                                 |
| `overlord.replicaCount`                  | overlord node replicas (deployment)                     | `1`                                        |
| `overlord.port`                          | port of overlord component                              | `8081`                                     |
| `overlord.serviceType`                   | service type for service                                | `ClusterIP`                                |
| `overlord.serviceAccount.create`         | Create a service account for overlord service           | `true`                                     |
| `overlord.serviceAccount.name`           | Service account name                                    | Derived from the name of service           |
| `overlord.serviceAccount.annotations`    | Annotations applied to created service account          | `{}`                                       |
| `overlord.serviceAccount.labels`         | Labels applied to created service account               | `{}`                                       |
| `overlord.serviceAccount.automountServiceAccountToken` | Automount API credentials for the Service Account | `true`                             |
| `overlord.resources`                     | overlord node resources requests & limits               | `{}`                                       |
| `overlord.podAnnotations`                | overlord Deployment annotations                         | `{}`                                       |
| `overlord.nodeSelector`                  | node labels for overlord pod assignment                 | `{}`                                       |
| `overlord.tolerations`                   | overlord tolerations                                    | `[]`                                       |
| `overlord.config`                         | overlord private config such as `JAVA_OPTS`              |                                            |
| `overlord.affinity`                       | overlord affinity policy                                 | `{}`                                       |
| `overlord.ingress.enabled`               | enable ingress                                          | `false`                                    |
| `overlord.ingress.hosts`                 | hosts for the overlord api                              | `[ "chart-example.local" ]`                |
| `overlord.ingress.path`                  | path of the overlord api                                | `/`                                        |
| `overlord.ingress.annotations`           | annotations for the overlord api ingress                | `{}`                                       |
| `overlord.ingress.tls`                   | TLS configuration for the ingress                        | `[]`                                       |
| `historical.enabled`                     | enable historical                                       | `true`                                     |
| `historical.name`                        | historical component name                               | `historical`                               |
| `historical.replicaCount`                | historical node replicas (statefulset)                  | `1`                                        |
| `historical.port`                        | port of historical component                            | `8083`                                     |
| `historical.serviceType`                 | service type for service                                | `ClusterIP`                                |
| `historical.serviceAccount.create`       | Create a service account for historical service         | `true`                                     |
| `historical.serviceAccount.name`         | Service account name                                    | Derived from the name of service           |
| `historical.serviceAccount.annotations`  | Annotations applied to created service account          | `{}`                                       |
| `historical.serviceAccount.labels`       | Labels applied to created service account               | `{}`                                       |
| `historical.serviceAccount.automountServiceAccountToken` | Automount API credentials for the Service Account | `true`                           |
| `historical.resources`                   | historical node resources requests & limits             | `{}`                                       |
| `historical.livenessProbeInitialDelaySeconds`  | historical node liveness probe initial delay in seconds  | `60`                                |
| `historical.readinessProbeInitialDelaySeconds` | historical node readiness probe initial delay in seconds | `60`                                |
| `historical.podAnnotations`              | historical Deployment annotations                       | `{}`                                       |
| `historical.nodeSelector`                | node labels for historical pod assignment               | `{}`                                       |
| `historical.securityContext`             | custom security context for historical containers       | `{ fsGroup: 1000 }`                        |
| `historical.tolerations`                 | historical tolerations                                  | `[]`                                       |
| `historical.config`                       | historical node private config such as `JAVA_OPTS`       |                                            |
| `historical.persistence.enabled`         | historical persistent enabled/disabled                  | `true`                                     |
| `historical.persistence.size`            | historical persistent volume size                       | `4Gi`                                      |
| `historical.persistence.storageClass`    | historical persistent volume Class                      | `nil`                                      |
| `historical.persistence.accessMode`      | historical persistent Access Mode                       | `ReadWriteOnce`                            |
| `historical.antiAffinity`                 | historical anti-affinity policy                          | `soft`                                     |
| `historical.nodeAffinity`                 | historical node affinity policy                          | `{}`                                       |
| `historical.ingress.enabled`             | enable ingress                                          | `false`                                    |
| `historical.ingress.hosts`               | hosts for the historical api                            | `[ "chart-example.local" ]`                |
| `historical.ingress.path`                | path of the historical api                              | `/`                                        |
| `historical.ingress.annotations`         | annotations for the historical api ingress              | `{}`                                       |
| `historical.ingress.tls`                 | TLS configuration for the ingress                        | `[]`                                       |
| `middlemanager.enabled`                  | enable middlemanager                                    | `true`                                     |
| `middlemanager.name`                     | middlemanager component name                            | `middlemanager`                            |
| `middlemanager.replicaCount`             | middlemanager node replicas (statefulset)               | `1`                                        |
| `middlemanager.port`                     | port of middlemanager component                         | `8091`                                     |
| `middlemanager.serviceType`              | service type for service                                | `ClusterIP`                                |
| `middlemanager.serviceAccount.create`    | Create a service account for middlemanager service      | `true`                                     |
| `middlemanager.serviceAccount.name`      | Service account name                                    | ``                                         |
| `middlemanager.serviceAccount.annotations` | Annotations applied to created service account        | `{}`                                       |
| `middlemanager.serviceAccount.labels`    | Labels applied to created service account               | `{}`                                       |
| `middlemanager.serviceAccount.automountServiceAccountToken` | Automount API credentials for the Service Account | `true`                        |
| `middlemanager.resources`                | middlemanager node resources requests & limits          | `{}`                                       |
| `middlemanager.podAnnotations`           | middlemanager Deployment annotations                    | `{}`                                       |
| `middlemanager.nodeSelector`             | Node labels for middlemanager pod assignment            | `{}`                                       |
| `middlemanager.securityContext`          | custom security context for middlemanager containers    | `{ fsGroup: 1000 }`                        |
| `middlemanager.tolerations`              | middlemanager tolerations                               | `[]`                                       |
| `middlemanager.config`                    | middlemanager private config such as `JAVA_OPTS`         |                                            |
| `middlemanager.persistence.enabled`      | middlemanager persistent enabled/disabled               | `true`                                     |
| `middlemanager.persistence.size`         | middlemanager persistent volume size                    | `4Gi`                                      |
| `middlemanager.persistence.storageClass` | middlemanager persistent volume Class                   | `nil`                                      |
| `middlemanager.persistence.accessMode`   | middlemanager persistent Access Mode                    | `ReadWriteOnce`                            |
| `middlemanager.antiAffinity`              | middlemanager anti-affinity policy                       | `soft`                                     |
| `middlemanager.nodeAffinity`              | middlemanager node affinity policy                       | `{}`                                       |
| `middlemanager.autoscaling.enabled`      | enable horizontal pod autoscaling                       | `false`                                    |
| `middlemanager.autoscaling.minReplicas`  | middlemanager autoscaling min replicas                  | `2`                                        |
| `middlemanager.autoscaling.maxReplicas`  | middlemanager autoscaling max replicas                  | `5`                                        |
| `middlemanager.autoscaling.metrics`      | middlemanager autoscaling metrics                       | `{}`                                       |
| `middlemanager.ingress.enabled`          | enable ingress                                          | `false`                                    |
| `middlemanager.ingress.hosts`            | hosts for the middlemanager api                         | `[ "chart-example.local" ]`                |
| `middlemanager.ingress.path`             | path of the middlemanager api                           | `/`                                        |
| `middlemanager.ingress.annotations`      | annotations for the middlemanager api ingress           | `{}`                                       |
| `middlemanager.ingress.tls`              | TLS configuration for the ingress                        | `[]`                                       |
| `router.enabled`                         | enable router                                           | `false`                                    |
| `router.name`                            | router component name                                   | `router`                                   |
| `router.replicaCount`                    | router node replicas (deployment)                       | `1`                                        |
| `router.port`                            | port of router component                                | `8888`                                     |
| `router.serviceType`                     | service type for service                                | `ClusterIP`                                |
| `router.serviceAccount.create`           | Create a service account for router service             | `true`                                     |
| `router.serviceAccount.name`             | Service account name                                    | Derived from the name of service           |
| `router.serviceAccount.annotations`      | Annotations applied to created service account          | `{}`                                       |
| `router.serviceAccount.labels`           | Labels applied to created service account               | `{}`                                       |
| `router.serviceAccount.automountServiceAccountToken` | Automount API credentials for the Service Account | `true`                               |
| `router.resources`                       | router node resources requests & limits                 | `{}`                                       |
| `router.podAnnotations`                  | router Deployment annotations                           | `{}`                                       |
| `router.nodeSelector`                    | node labels for router pod assignment                   | `{}`                                       |
| `router.tolerations`                     | router tolerations                                      | `[]`                                       |
| `router.config`                           | router private config such as `JAVA_OPTS`                |                                            |
| `router.affinity`                         | router affinity policy                                   | `{}`                                       |
| `router.ingress.enabled`                 | enable ingress                                          | `false`                                    |
| `router.ingress.hosts`                   | hosts for the router api                                | `[ "chart-example.local" ]`                |
| `router.ingress.path`                    | path of the router api                                  | `/`                                        |
| `router.ingress.annotations`             | annotations for the router api ingress                  | `{}`                                       |
| `router.ingress.tls`                     | TLS configuration for the ingress                        | `[]`                                       |
| `prometheus.enabled`                     | Support scraping from prometheus                     | `false`                                                       |
| `prometheus.port`                        | expose prometheus port                               | `9090`                                                        |
| `prometheus.annotation`                  | pods annotation to notify prometheus scraping        | `{prometheus.io/scrape: "true", prometheus.io/port: "9090"}`  |

Full and up-to-date documentation can be found in the comments of the `values.yaml` file.
