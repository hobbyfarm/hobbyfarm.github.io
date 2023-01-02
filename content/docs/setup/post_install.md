+++
title = "Post-Install Setup"
weight = 3

+++

## Initial Admin User

Create a new user via the user-UI (Register a user).
Verify the creation of the user with `kubectl get users -n <namespace>`.

See [user documentation]({{< ref "/docs/architecture/resources/user.md" >}}) on how to give this user access to the admin dashboard.

By default a Role `hobbyfarm-admin` is created, granting access to all resources.

To bind your user to this Role create the following RoleBinding

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
  name: <id of your created user>
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: hobbyfarm-admin
  apiGroup: rbac.authorization.k8s.io
```

## Initial VM Template

Before you can provision virtual machines, you must first define a `VirtualMachineTemplate`. A virtual machine template is a provider-agnostic representation of a template for a VM. It is used to schedule resources across different providers using a common definition. 

See [this link]({{< ref "/docs/architecture/resources/virtualmachinetemplate.md" >}}) for a sample VMT and more information on that resource.

## 