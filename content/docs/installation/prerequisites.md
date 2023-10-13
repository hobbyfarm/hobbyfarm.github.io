+++
title = "Prerequisites"
description = "Required prerequisites for deploying HobbyFarm."
weight = 1
+++

The following prerequisites are required for installing HobbyFarm. Please ensure that all prerequisites are met before proceeding with the installation. Failure to do so may result in an unsuccessful installation.

## Helm

Helm v3 is required for deploying HobbyFarm. Please refer to the [Helm documentation](https://helm.sh/docs/intro/install/) for instructions on installing Helm.

## Kubernetes

HobbyFarm is a Kubernetes-native application, using [custom resources](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) and controllers to manage the application. To run HobbyFarm, a Kubernetes cluster with an installed ingress controller is required. The following list shows the supported Kubernetes versions for each HobbyFarm version.

| HobbyFarm Release | Release Date | K8s Version (up to) |
| :--- | :--- | :--- |
| [hobbyfarm-3.0.1](https://github.com/hobbyfarm/hobbyfarm/releases/tag/hobbyfarm-3.0.1) | July 2023 | v1.25.x |
| [hobbyfarm-2.0.9](https://github.com/hobbyfarm/hobbyfarm/releases/tag/hobbyfarm-2.0.9) | June 2023 | v1.24.x |
| [hobbyfarm-1.0.0](https://github.com/hobbyfarm/hobbyfarm/releases/tag/hobbyfarm-1.0.0) | January 2022 | v1.23.x |

### `Role-Based Access Control (RBAC)`

HobbyFarm requires an account with the following permissions for installation. For simplicity, the use of an account with `cluster-admin` role binding is recommended.

|API Group|Resource|Verbs|
|---------|--------|-----|
|apiextensions.k8s.io|CustomResourceDefinitions|Get, Create, Update|
|apps|Deployments|Get, Create, Update, Delete|
|core|ConfigMaps, Secrets, Services|Get, Create, Update, Delete|
|networking.k8s.io|Ingresses|Get, Create, Update, Delete|
|rbac.authorization.k8s.io|ClusterRoles, ClusterRoleBindings, Roles, RoleBindings|Get, Create, Update, Delete|


### `ClusterRole Manifest`

If an account is not available with full cluster access, or a `cluster-admin` role does not exist, the following `ClusterRole` manifest can be created and bound to an account.

> **NOTE:** The following manifest maps to the permissions listed in the table above.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
    name: hobbyfarm-install
rules:
    - apiGroups: ["apiextensions.k8s.io"]
      resources: ["customresourcedefinitions"]
      verbs: ["get", "create", "update"]
    - apiGroups: ["apps"]
      resources: ["deployments"]
      verbs: ["get", "create", "update", "delete"]
    - apiGroups: [""]
      resources: ["configmaps", "secrets", "services"]
      verbs: ["get", "create", "update", "delete"]
    - apiGroups: ["networking.k8s.io"]
      resources: ["ingresses"]
      verbs: ["get", "create", "update", "delete"]
    - apiGroups: ["rbac.authorization.k8s.io"]
      resources: ["clusterroles", "clusterrolebindings", "roles", "rolebindings"]
      verbs: ["get", "create", "update", "delete"]
```