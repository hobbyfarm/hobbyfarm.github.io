+++
title = "Prerequisites"
description = "Required prerequisites for installing HobbyFarm."
weight = 1
+++

The following prerequisites are required for installing HobbyFarm. Please ensure that all prerequisites are met before proceeding with the installation. Failure to do so may result in an unsuccessful installation.

## Helm

Helm v3 is required for installing HobbyFarm. Please refer to the [Helm documentation](https://helm.sh/docs/intro/install/) for installation instructions.

## Kubernetes

In order to run HobbyFarm, a Kubernetes cluster with an installed ingress controller is required. The following list shows the supported Kubernetes versions for each HobbyFarm version.


| HobbyFarm Version | Kubernetes Version (up to) |
|-------------------|----------------------------|
|v3.0.1|v1.25.x|
|v2.0.1|v1.24.x|
|v1.0x|v1.23.x|

### Role-Based Access Control (RBAC)

HobbyFarm requires an account with the following permissions for installation. For simplicity, the use of an account with `cluster-admin` role binding is recommended.

|API Group|Resource|Verbs|
|---------|--------|-----|
|apiextensions.k8s.io|CustomResourceDefinitions|Get, Create, Update|
|apps|Deployments|Get, Create, Update, Delete|
|core|ConfigMaps, Secrets, Services|Get, Create, Update, Delete|
|networking.k8s.io|Ingresses|Get, Create, Update, Delete|
|rbac.authorization.k8s.io|ClusterRoles, ClusterRoleBindings, Roles, RoleBindings|Get, Create, Update, Delete|


### ClusterRole Manifest

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