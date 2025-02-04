---
title: FluxCD GitOps
---

This addon is built based [FluxCD](https://fluxcd.io/)

## install

```shell
vela addon enable fluxcd
```

## Definitions

The following definitions will be enabled after the installation of fluxcd addon.

- [helm](https://kubevela.io/docs/end-user/components/helm) helps to deploy a helm chart from git repo, helm repo or S3 compatible bucket.

- [kustomize](https://kubevela.io/docs/end-user/components/kustomize) helps to deploy a kustomize style artifact and GitOps capability to watch changes from git repo or image registry.

## Note

- In this addon, there are five controllers to be installed by default

|CONTROLLER NAME                |DEFINITION NAME                         |DEFINITION TYPE           |DEFINITION DESCRIPTION|
| :----:        |    :----:   |          :----: | ---|
|helm-controller                |config-helm-repository                  |ComponentDefinition       |Config information to authenticate helm chart repository|
|helm-controller                |helm                                    |ComponentDefinition       |helm release is a group of K8s resources from either gitrepository or helm repo|
|kustomize-controller           |kustomize                               |ComponentDefinition       |kustomize can fetching, building, updating and applying Kustomize manifests from git repo|
|kustomize-controller           |kustomize-json-patch                    |TraitDefinition           |A list of JSON6902 patch to selected target|
|kustomize-controller           |kustomize-patch                         |TraitDefinition           |A list of StrategicMerge or JSON6902 patch to selected target|
|kustomize-controller           |kustomize-strategy-merge                |TraitDefinition           |A list of strategic merge to kustomize config

- Source controller
  - The source-controller is a Kubernetes operator, specialised in artifacts acquisition from external sources such as Git, Helm repositories and S3 buckets. The source-controller implements the source.toolkit.fluxcd.io API and is a core component of the GitOps toolkit.
  - ![overview](https://github.com/fluxcd/source-controller/blob/main/docs/diagrams/source-controller-overview.png)

- Image (metadata) reflector controller
  - This is a controller that reflects container image metadata into a Kubernetes cluster. It pairs with the image update automation controller to drive automated config updates.

- Image automation controller
  - This controller automates updates to YAML when new container images are available.
  - Its sibling, image-reflector-controller, scans container image repositories and reflects the metadata in Kubernetes resources. This controller reacts to that image metadata by updating YAML files in a git repository, and committing the changes.

- kustomize-controller
  - The kustomize-controller is a Kubernetes operator, specialized in running continuous delivery pipelines for infrastructure and workloads defined with Kubernetes manifests and assembled with Kustomize.
  - ![overview](https://github.com/fluxcd/kustomize-controller/blob/main/docs/diagrams/kustomize-controller-overview.png)

- helm-controller
  - The helm-controller is a Kubernetes operator, allowing one to declaratively manage Helm chart releases. It is part of a composable GitOps toolkit and depends on source-controller to acquire the Helm charts from Helm repositories.
  - The desired state of a Helm release is described through a Kubernetes Custom Resource named HelmRelease. Based on the creation, mutation or removal of a HelmRelease resource in the cluster, Helm actions are performed by the operator.
  - ![overview](https://github.com/fluxcd/helm-controller/blob/main/docs/diagrams/helm-controller-overview.png)
