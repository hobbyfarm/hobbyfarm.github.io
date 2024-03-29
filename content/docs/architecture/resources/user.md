+++
title = "User"
description = "A user or administrator of the HobbyFarm platform."
+++

Users are represented in HobbyFarm via the `User` resource. Users can be created automatically via the registration page or manually via a manifest.

## Kubernetes Commands
The following commands are useful for managing User resources in Kubernetes.

```bash
## Get a list of all Users
kubectl get users -n hobbyfarm-system

## Create a User from a YAML manifest
kubectl apply -f {userManifest} -n hobbyfarm-system

## Edit a User
kubectl edit user {userName} -n hobbyfarm-system

## Backup a User to a YAML manifest
kubectl get user {userName} -n hobbyfarm-system -o yaml > {userManifest}

## Delete a User
kubectl delete user {userName} -n hobbyfarm-system
```

## Example User Manifest
The following is an example of a User resource in Kubernetes.

```yaml
apiVersion: hobbyfarm.io/v2
kind: User
metadata:
    name: example-user
    namesapce: hobbyfarm-system
spec:
    email: user@example.com
    password: $6p$01$LJsdflisn3d9jrlkjdf
    access_codes:
    - event01
    - demo99
    settings:
      ctr_enabled: "true"
      ctxAccessCode: example-access-code
      terminal_fontSize: "16"
      terminal_theme: Solarized_Dark_Higher_Contrast
```

## Configuration

### `email`
Determines the username for the user. During original development of HobbyFarm it was assumed that all users would login using an email address and password. However, no validation in HobbyFarm or any UI forces an email to be used for this field.

> **NOTE:** It is acceptable to treat this field as a synonym for username.

### `password`

This is the bcrypt'ed password of the user. This field is set when a user registers in the end-user UI. It can be changed via the admin UI. 

### `access_codes`

These are the access codes (not resource names, but actual codes) that the user has entered in the end-user UI. If matched to an access code these will grant access to content in HobbyFarm. 

### `settings`

Settings of users are stored in their ressource as a `map[string]string`. The following options are currently available:

|Key|Default|Purpose|
|---|-------|-------|
|`ctr_enabled`|`true`|If CTRs enabled|
|`ctxAccessCode`|`<empty-string>`|Selected Context AccessCode|
|`terminal_fontSize`|`16`|Terminal Font-Size|
|`terminal_theme`|`default`|Terminal Theme|

## Admin Access
By default users do not have access to the admin dashboard.
Granting access to resources works via kubernetes native Roles and RoleBindings

You can create new Roles via the admin dashboard or use one of the predefined Roles created by gargantua when providing the flag `-installrbacroles`

A Role granting access to all Resources would look like the following:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
    labels:
        rbac.hobbyfarm.io/managed: "true"
    name: hobbyfarm-admin
    namespace: {{ .Release.Namespace }}
rules:
- apiGroups: ["hobbyfarm.io"]
  resources: ["*"]
  verbs: ["*"]
- apiGroups: ["rbac.authorization.k8s.io"]
  resources: ["roles", "rolebindings"]
  verbs: ["*"]
```

To grant a user this role you would give him access via the Admin-UI or create a RoleBinding:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
    labels:
        rbac.hobbyfarm.io/managed: "true"
    name: hobbyfarm-admin-rolebinding
    namespace: {{ .Release.Namespace }}
subjects:
- kind: User
  name: <id-of-user>
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: hobbyfarm-admin
  apiGroup: rbac.authorization.k8s.io
```
