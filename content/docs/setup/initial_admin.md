+++
title = "Admin Account"
weight = 3
+++

Once you have [installed HobbyFarm](/docs/setup/installation), an initial administrator account must be created. This account will be used to manage the HobbyFarm platform.

## Step 1: Register a User

Visit the newly deployed HobbyFarm UI at `https://learn.{domain}.com` and register a new user.

> **NOTE:** If a different URL for the end-user learning interface was used, visit that URL instead.

## Step 2: Verify the User

As part of the deployment of HobbyFarm, new API resources are created in the cluster. One of these resources is the `User` resource. This resource is used to authenticate users to the HobbyFarm platform.

Verify the user has been created in the Kubernetes `User` resource.
```bash
kubectl get users -n hobbyfarm-system
```

> **NOTE:** Make note of the dynamically generated `NAME` of the user, as it will be used in the next step.

## Step 3: Bind the User

By default, a Role named `hobbyfarm-admin` is created, granting access to all HobbyFarm resources. Review this role by running the following command:

```bash
kubectl get role hobbyfarm-admin -n hobbyfarm-system -o yaml
```

Bind your user to this Role by creating the following RoleBinding, updating the `name` field with the dynamically generated `NAME` from Step 2:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
    name: hobbyfarm-admin-rolebinding
    namespace: hobbyfarm-system
subjects:
- kind: User
  name: u-xxxxxxxxxx    ## Use the dynamically generated NAME from Step 2
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: hobbyfarm-admin
  apiGroup: rbac.authorization.k8s.io
```

> **NOTE:** The RoleBinding makes use of the role name `hobbyfarm-admin` which is created when HobbyFarm is deployed.

> **NOTE:** Multiple users can be added to this RoleBinding by adding additional `subjects`.

## Additional Users

See the [User documentation](/docs/architecture/resources/user) for more information on user management.
