+++
title = "Prerequisites"

weight = 1
+++

Generally speaking, all that is required to run HobbyFarm is a Kubernetes cluster with an ingress controller installed. 

| HobbyFarm Version      | Supported Kubernetes Version (up to) |
| ----------- | ----------- |
|v2.0.1|v1.24.x|
|v2.0.0|v1.24.x|
|v1.0x|v1.23.x|

## Helm

Helm 3+ is required for installing HobbyFarm. 

## RBAC

In order to install HobbyFarm, it is required that you use an account with the following permissions:

|API Group|Resource|Verbs|
|---------|--------|-----|
|apiextensions.k8s.io|CustomResourceDefinitions|Get, Create, Update|
|apps|Deployments|Get, Create, Update, Delete|
|core|ConfigMaps, Secrets, Services|Get, Create, Update, Delete|
|networking.k8s.io|Ingresses|Get, Create, Update, Delete|
|rbac.authorization.k8s.io|ClusterRoles, ClusterRoleBindings, Roles, RoleBindings|Get, Create, Update, Delete|

It is easiest to use a `ClusterAdmin` account to accomplish the above, but if you need to use a specific role, here is a definition for your convenience.

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