## Introduction
# Chaoskube Helm Chart

[chaoskube](https://github.com/linki/chaoskube) periodically kills random pods in your Kubernetes cluster.

## Note 
The original work for this helm chart is present @ [Helm Charts]( https://github.com/helm/charts) Based on the [chaoskube]( https://github.com/helm/charts/tree/master/stable/chaoskube) chart

## TL;DR;

```console
$ helm install stable/chaoskube
```
## Resources Required
The chart deploys pods consuming minimum resources as specified in the resources configuration parameter (default: Memory: 200Mi, CPU: 100m)

## Chart Details
Chaoskube Helm Chart which periodically kills random pods in your Kubernetes cluster.

## Prerequisites 
-Tiller 2.6.0 or later
 
## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm install --name my-release stable/chaoskube
```

The command deploys chaoskube on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the my-release deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

By default `chaoskube` runs in dry-run mode so it doesn't actually kill anything.
If you're sure you want to use it run `helm` with:

```console
$ helm install stable/chaoskube --set dryRun=false
```

| Parameter                 | Description                                         | Default                           |
|---------------------------|-----------------------------------------------------|-----------------------------------|
| `name`                    | container name                                      | chaoskube                         |
| `image`                   | docker image                                        | quay.io/linki/chaoskube           |
| `imageTag`                | docker image tag                                    | v0.8.0                            |
| `replicas`                | number of replicas to run                           | 1                                 |
| `interval`                | interval between pod terminations                   | 10m                               |
| `labels`                  | label selector to filter pods by                    | "" (matches everything)           |
| `annotations`             | annotation selector to filter pods by               | "" (matches everything)           |
| `namespaces`              | namespace selector to filter pods by                | "" (all namespaces)               |
| `dryRun`                  | don't kill pods, only log what would have been done | true                              |
| `timezone`                | Set timezone for running actions (Optional)         | "" (UTC)                          |
| `excludedWeekdays`        | Set Days of the Week to avoid actions (Optional)    | "" (Don't skip any weekdays)      |
| `excludedTimesOfDay`      | Set Time Range to avoid actions (Optional)          | "" (Don't skip any times of day)  |
| `excludedDaysOfYear`      | Set Days of the Year to avoid actions (Optional)    | "" (Don't skip any days)          |
| `resources.cpu`           | cpu resource requests and limits                    | 10m                               |
| `resources.memory`        | memory resource requests and limits                 | 16Mi                              |
| `rbac.create`             | create rbac service account and roles               | false                             |
| `rbac.serviceAccountName` | name of serviceAccount to use when create is false  | default                           |
| `nodeSelector`            | node labels for pod assignment                      | `{}`                              |

Setting label and namespaces selectors from the shell can be tricky but is possible (example with zsh):

```console
$ helm install \
  --set labels='app=mate\,stage!=prod',namespaces='!kube-system\,!production' \
  stable/chaoskube --debug --dry-run | grep -A7 args
    args:
    - --interval=10m
    - --labels=app=foo,stage!=prod
    - --namespaces=!kube-system,!production
    - --timezone=America/New_York
    - --excludedWeekdays="Sat,Tue"
    - --excludedTimesOfDay="12:00-18:00"
    - --excludedDaysOfYear="Apr1,Dec24"
```
## Limitations
