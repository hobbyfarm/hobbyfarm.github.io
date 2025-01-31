+++
title = "Costs"
description = "Configure cost racking for resources."
weight = 2
+++

HobbyFarm allows cost tracking for a group of resources, known as a cost group. By default, costs are tracked for Virtual Machines and can be defined for VM Templates in the Admin-UI, as explained in the [VM Template section](docs/configuration/vmtemplates).

Cost tracking works natively for default Kubernetes resource, but it is also possible to track costs for other resources.

## Track costs for other resources

To track costs for resources, the following rules must be followed:

> **Rule:** Cost tracking is only supported for resources within the same namespace where HobbyFarm is deployed.
> 
> **Rule:** Cost tracking for a resource begins at its creation and ends upon its deletion.

### Enable cost tracking or non default Kubernetes resources

Adapt the `values.yaml` in the helm chart and add your custom resource in the `cost.trackableResources` list. Then deploy the helm chart.

> **NOTE:** Please visit the [Helm Options](/docs/appendix/helm_options) page for more information.

Cost tracking works natively for default Kubernetes resources.

### Tracking costs for resources

The tracking works by adding the following labels to your resource

| Label Name                         | Description                                                                                    | Example         |
|------------------------------------|------------------------------------------------------------------------------------------------|-----------------|
| **_hobbyfarm.io/cost-group_**      | The name of the cost group.                                                                    | `my-cost-group` | 
| **_hobbyfarm.io/cost-base-price_** | The base price for the resource. The base price must be a valid floating-point number.         | `0.00324`       |
| **_hobbyfarm.io/cost-time-unit_**  | The optional time unit for the cost tracking. The time unit must be seconds, minutes or hours. | `seconds`       |

When a resource with the required labels is created, a corresponding Cost resource is generated, including the `creation_unix_timestamp` property. Upon deletion of the resource, the Cost resource is updated to include the `deletion_unix_timestamp` property.   

 










