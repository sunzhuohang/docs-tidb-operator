---
title: 销毁 Kubernetes 上的 TiDB 集群
summary: 介绍如何销毁 Kubernetes 集群上的 TiDB 集群。
category: how-to
---

# 销毁 Kubernetes 上的 TiDB 集群

本文描述了如何销毁 Kubernetes 集群上的 TiDB 集群。

## 销毁使用 TidbCluster 管理的 TiDB 集群

要销毁使用 TidbCluster 管理的 TiDB 集群，执行以下命令：

{{< copyable "shell-regular" >}}

```shell
kubectl delete tc ${cluster_name} -n ${namespace}
```

如果集群中通过 `TidbMonitor` 部署了监控，要删除监控组件，可以执行以下命令：

{{< copyable "shell-regular" >}}

```shell
kubectl delete tidbmonitor ${tidb_monitor_name} -n ${namespace}
```

## 销毁使用 Helm 管理的 TiDB 集群

要销毁使用 Helm 管理的 TiDB 集群，执行以下命令：

{{< copyable "shell-regular" >}}

```shell
helm delete ${cluster_name} --purge
```

## 清除数据

上述销毁集群的命令只是删除运行的 Pod，数据仍然会保留。如果你不再需要那些数据，可以通过下面命令清除数据：

> **警告：**
>
> 下列命令会彻底删除数据，务必考虑清楚再执行。

{{< copyable "shell-regular" >}}

```shell
kubectl delete pvc -n ${namespace} -l app.kubernetes.io/instance=${cluster_name},app.kubernetes.io/managed-by=tidb-operator
```

{{< copyable "shell-regular" >}}

```shell
kubectl get pv -l app.kubernetes.io/namespace=${namespace},app.kubernetes.io/managed-by=tidb-operator,app.kubernetes.io/instance=${cluster_name} -o name | xargs -I {} kubectl patch {} -p '{"spec":{"persistentVolumeReclaimPolicy":"Delete"}}'
```
