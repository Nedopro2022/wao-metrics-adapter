# WAO Metrics Adapter

A metrics adapter for Kubernetes Metrics APIs that exposes custom metrics for WAO.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Overview](#overview)
- [Getting Started](#getting-started)
  - [Installation](#installation)
  - [Fetching Metrics](#fetching-metrics)
- [Development](#development)
  - [Components](#components)
- [Changelog](#changelog)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Overview

WAO Metrics Adapter exposes the following custom metrics for WAO:

- `inlet_temp`: Server inlet temperature in Celsius.
- `delta_p`: Server differential pressure in Pascal.

## Getting Started

### Installation

> [!NOTE]
> Make sure you have [wao-core](https://github.com/waok8s/wao-core) set up.

Install WAO Metrics Adapter.

```sh
kubectl apply -f https://github.com/waok8s/wao-metrics-adapter/releases/download/v1.27.0/wao-metrics-adapter.yaml
```

Wait for the pod to be ready.

```sh
kubectl wait pod $(kubectl get pods -n custom-metrics -l app=wao-metrics-adapter -o jsonpath="{.items[0].metadata.name}") -n custom-metrics --for condition=Ready
```

### Fetching Metrics

You can fetch the metrics using `kubectl get --raw` command.

```sh
# Your node name
NODE=worker-1

# Inlet temperature
kubectl get --raw "/apis/custom.metrics.k8s.io/v1beta2/nodes/$NODE/inlet_temp"
# Differential pressure
kubectl get --raw "/apis/custom.metrics.k8s.io/v1beta2/nodes/$NODE/delta_p"
```

Or you can use client libraries to fetch the metrics.

- `k8s.io/metrics/pkg/client/custom_metrics` has the official client
- `github.com/waok8s/wao-core/pkg/client` has our cached client


## Development

This project is using [custom-metrics-apiserver](https://github.com/kubernetes-sigs/custom-metrics-apiserver), which is a library based on [Kubernetes API Aggregation Layer](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/apiserver-aggregation/).

### Components

- `pkg/provider`: Custom metrics provider.

## Changelog

Versioning: we use the same major.minor as Kubernetes, and the patch is our own.

- What comes next:
  - TBD
- 2023-xx-xx `v1.27.0`
  - First release.
  - `provider` Add the custom metrics provider.

## License

Copyright 2023 Bitmedia, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
