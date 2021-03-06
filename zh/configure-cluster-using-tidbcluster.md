---
title: 通过 TidbCluster 配置 TiDB 集群
summary: 介绍如何使用 TidbCluster 配置 TiDB 集群。
category: how-to
---

# 通过 TidbCluster 配置 TiDB 集群

TidbCluster 文件支持在其上面直接配置 TiDB/TiKV/PD 的配置选项，本篇文章将介绍如何在 TidbCluster 上配置参数。目前 Operator 1.1 版本支持了 TiDB 集群 v3.1 版本参数。针对各组件配置参数，请参考 PingCAP 官方文档。

## 配置 TiDB 配置参数

你可以通过 `TidbCluster.Spec.Tidb.Config` 来配置 TiDB 配置参数，以下是一个例子。获取所有可以配置的 TiDB 配置参数，请参考 [TiDB 配置文档](https://pingcap.com/docs-cn/v3.1/reference/configuration/tidb-server/configuration-file/)。

> **注意：**
>
> 为了兼容 `helm` 部署，如果你是通过 CR 文件部署 TiDB 集群，即使你不设置 Config 配置，也需要保证 `Config: {}` 的设置，从而避免 TiDB 组件无法正常启动。

```yaml
apiVersion: pingcap.com/v1alpha1
kind: TidbCluster
metadata:
  name: basic
spec:
....
  tidb:
    image: pingcap.com/tidb:v3.1.0
    imagePullPolicy: IfNotPresent
    replicas: 1
    service:
      type: ClusterIP
    config:
      split-table: true
      oom-action: "log"
    requests:
      cpu: 1
```

## 配置 TiKV 配置参数

你可以通过 `TidbCluster.Spec.Tikv.Config` 来配置 TiKV 配置参数，以下是一个例子。获取所有可以配置的 TiKV 配置参数，请参考 [TiKV 配置文档](https://pingcap.com/docs-cn/v3.1/reference/configuration/tikv-server/configuration-file/)

> **注意：**
>
> 为了兼容 `helm` 部署，如果你是通过 CR 文件部署 TiDB 集群，即使你不设置 Config 配置，也需要保证 `Config: {}` 的设置，从而避免 TiKV 组件无法正常启动。

```yaml
apiVersion: pingcap.com/v1alpha1
kind: TidbCluster
metadata:
  name: basic
spec:
....
  tikv:
    image: pingcap.com/tikv:v3.1.0
    config:
      grpc-concurrenc: 4
      sync-log: true
    replicas: 1
    requests:
      cpu: 2
```

## 配置 PD 配置参数

你可以通过 `TidbCluster.Spec.Pd.Config` 来配置 PD 配置参数，以下是一个例子。获取所有可以配置的 PD 配置参数，请参考 [PD 配置文档](https://pingcap.com/docs-cn/v3.1/reference/configuration/pd-server/configuration-file/)

> **注意：**
>
> 为了兼容 `helm` 部署，如果你是通过 CR 文件部署 TiDB 集群，即使你不设置 Config 配置，也需要保证 `Config: {}` 的设置，从而避免 PD 组件无法正常启动。

```yaml
apiVersion: pingcap.com/v1alpha1
kind: TidbCluster
metadata:
  name: basic
spec:
.....
  pd:
    image: pingcap.com/pd:v3.1.0
    config:
      format: "format"
      disable-timestamp: false
```
