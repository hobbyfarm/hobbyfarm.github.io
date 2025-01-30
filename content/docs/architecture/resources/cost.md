+++
title = "Cost"
description = "Entity to store resource costs."
+++

The `Cost` resource in HobbyFarm tracks resource cost for one or more kubernetes resources. A single entity represents a cost group.


## Kubernetes Commands
The following commands are useful for managing Cost resources in Kubernetes.

```bash
## Get a list of all Costs
kubectl get costs -n hobbyfarm-system

## Delete a Cost
kubectl delete cost {costName} -n hobbyfarm-system
```

## Example Cost Manifest
The following is an example of a Cost resource in Kubernetes.

```yaml
# Cost group with one resource which is still running.
apiVersion: hobbyfarm.io/v1
kind: Cost
metadata:
  name: test-cost-group
  namespace: hobbyfarm-system
spec:
  cost_group: test-cost-group
  resources:
    - base_price: 0.001 # float64
      creation_unix_timestamp: 1735899136
      id: vm-neuc2clllz # references the resource ID
      kind: VirtualMachine # references the resource kind
      time_unit: hours

---

# Cost group with one resource which is terminated.
apiVersion: hobbyfarm.io/v1
kind: Cost
metadata:
  name: test-cost-group
  namespace: hobbyfarm-system
spec:
  cost_group: test-cost-group
  resources:
    - base_price: 0.001
      creation_unix_timestamp: 1735899136
      deletion_unix_timestamp: 1735899999 # deletion timestamp set
      id: vm-neuc2clllz
      kind: VirtualMachine
      time_unit: hours



```

## Configuration

### `cost_group`
Represents the unique identifier for a cost group. 

### `resources`
A list of resources that are incurring or will incur costs within this cost group.

#### `base_price`
A floating-point value representing the base cost of a resource. This cost is applied for each defined time_unit.

#### `creation_unix_timestamp`
Records the creation time of a resource as a unix timestamp.

#### `deletion_unix_timestamp`
Indicates the deletion time of a resource as a Unix timestamp. If not provided, the resource is considered running.

#### `id`
A unique identifier for the tracked resource.

#### `kind`
Specifies the kind of the tracked resource.

#### `time_unit`
Defines the unit of time (seconds, minutes, or hours) used for cost calculation. The base_price is applied at each interval of the specified time_unit.



