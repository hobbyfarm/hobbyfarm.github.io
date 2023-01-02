+++
title = "User"
+++

Users are represented in HobbyFarm via the `User` resource. 

Here's an example of that resource:
```yaml
apiVersion: hobbyfarm.io/v2
kind: User
metadata:
    name: example-user
spec:
    id: example-user
    email: user@example.com
    password: $6p$01$LJsdflisn3d9jrlkjdf
    access_codes:
    - event01
    - demo99
```

## Configuration

Most user accounts are created through registration via the end-user UI. You should only need to manually create or adjust accounts for users that need access to the admin UI.

### `id`

Identifier for the user. Should be identical to the Kubernetes `metadata.name` field. Present for historical reasons, will be phased out in a future release. 

> This field is currently *required* in HobbyFarm. Architecturally it is considered deprecated and may be removed in a future release. For now, users must continue to set this field. 

### `email`

This field determines the username for the user. During original development of HobbyFarm it was assumed that all users would login using an email address and password. However, no validation in HobbyFarm or any UI forces an email to be used for this field. 

*Therefore it is acceptable to treat this field as a synonym for username.*

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
