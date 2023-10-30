+++
title = "Roles"
description = "Create or manage Kubernetes roles used by HobbyFarm."
weight = 5
+++

In Kubernetes, a [Role](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole) is a set of permissions that can be assigned to a user or group of users. A `Role` can be assigned to a user or group of HobbyFarm users via a `RoleBinding`. HobbyFarm provides an interface for creating and managing HobbyFarm specific `Roles` via the Admin-UI.

To access the Roles page, log into the HobbyFarm Admin-UI and click the `Configuration` option in the top navigation menu. In the left navigation menu, click the `Roles` option.

## Creating a Role

To create a new `Role`, click on the `+NEW` button in the top left corner of the page under the `Roles` heading. A popup will appear with a form to fill out the details of the new `Role`.

![Role - Basic Information](/images/hobbyfarm-admin-role-create.png)

#### `Variables`
| Setting | Required | Description |
| --- | --- | --- |
| **_Role Name_** | Yes | The name of the `Role`. This name will be used to identify the `Role` in the HobbyFarm Admin-UI and API. |
| **_API Groups_** | Yes | The API groups to which the `Role` applies. Two API groups are available: `hobbyfarm.io` and `rbac.authorization.k8s.io`. |
| **_Resources_** | Yes | The resources to which the `Role` applies. Examples are `virtualmachines`, `environments`, `users`, and `scheduledevents`. Each of these are resources available in Kubernetes via the HobbyFarm deployment. |
| **_Verbs_** | Yes | The verbs to which the selected Resources applies. Examples are `get`, `list`, `create`, `update`, and `delete`. |

Once the information has been entered, click the `SAVE` button in the lower right corner to create the `Role`.

> **NOTE:** Roles must be applied using RoleBindings before they will take effect. The use of RoleBindings is outside the scope of this document. Please visit the [Kubernetes Role-Based Access Control (RBAC)](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) documentation for more information on RoleBindings.
