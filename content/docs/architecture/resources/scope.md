+++
title = "Scope"
description = "Defines a set of settings that are accessible by a specific group of users."
+++

A Scope is used to control access to settings in the HobbyFarm UI. For example, a setting that is only accessible by administrators would be assigned to the `admin-ui` scope.

## Kubernetes Commands
The following commands are useful for managing Scope resources in Kubernetes.

```bash
## Get a list of all Scopes
kubectl get scopes -n hobbyfarm-system

## Create a Scope from a YAML manifest
kubectl apply -f {scopeManifest} -n hobbyfarm-system

## Edit a Scope
kubectl edit scope {scopeName} -n hobbyfarm-system

## Backup a Scope to a YAML manifest
kubectl get scope {scopeName} -n hobbyfarm-system -o yaml > {scopeManifest}

## Delete a Scope
kubectl delete scope {scopeName} -n hobbyfarm-system
```

## Example Scope Manifests
The following shows an example of a Scope manifest in Kubernetes.

```yaml
## Public Scope
apiVersion: hobbyfarm.io/v1
displayName: Public
kind: Scope
metadata:
  name: public
  namespace: hobbyfarm-system

---

## User UI Scope
apiVersion: hobbyfarm.io/v1
displayName: User UI
kind: Scope
metadata:
  name: user-ui
  namespace: hobbyfarm-system

---

## Admin UI Scope
apiVersion: hobbyfarm.io/v1
displayName: Admin UI
kind: Scope
metadata:
  name: admin-ui
  namespace: hobbyfarm-system
```

## Default Scopes
The following scopes are available in HobbyFarm by default:

| Scope | Description |
| --- | --- |
| public | Settings which are accessible without registration. |
| user-ui | Settings which are accessible by registered users. |
| admin-ui | Settings which are accessible by administrators. |

## Use with Settings Resource
A Scope works in conjunction with the `hobbyfarm.io/setting-scope` label on a [Setting](/docs/architecture/resources/settings) resource. The label defines which scope a setting belongs to. For example, a setting that is assigned to the `admin-ui` scope would have the label `hobbyfarm.io/setting-scope: admin-ui`.

* Scopes can also be created by operators or any HobbyFarm compatible application. For example, an AWS Provisioning Operator could generate an `aws` scope.
* A Scope is applied to a Setting via a label using the key `hobbyfarm.io/setting-scope` with the value of the scope name. For example:
  * `hobbyfarm.io/setting-scope: public`
  * `hobbyfarm.io/setting-scope: user-ui`
  * `hobbyfarm.io/setting-scope: admin-ui`
