+++
title = "Environment"
+++

An `Environment` defines specific implementation details on how HobbyFarm connects to a provider for scheduling virtual machines. An environment resource contains configuration information such as where provider credentials are stored, what image to use when creating a VM, or how much capacity a provider has for a specific type of VM. 

Here's an example environment manifest:

```yaml
apiVersion: hobbyfarm.io/v1
kind: Environment
metadata:
    name: sample-environment
    namespace: hobbyfarm-system
spec:
    burst_capable: true
    burst_count_capacity: 
        example-template: 100
    capacity_mode: count
    count_capacity:
        example-template: 500
    display_name: sample-environment
    environment_specifics:
        cred_secret: sample-environment-creds
        region: us-east-1
        subnet: subnet-09823djk
        vpc_security_group_id: sg-09823iwedlijqd39
    provider: aws
    template_mapping:
        example-template:
            image: ami-098230498234
            size: t3.medium
    ws_endpoint: ws.your-hobbyfarm.io
```

## Configuration

### `burst_capable`

Bursting was the original name given to HobbyFarm's capability to dynamically provision VMs. Setting `spec.burst_capable` to `true` on an environment means that this environment is tolerant of dynamically created VMs. 

> This field may be removed in a future release of HobbyFarm. External provisioners generally do not distinguish between ahead-of-time (AOT) provisioning and dynamic or on-demand provisioning. Thus an environment may not need to define this property at all. To track this discussion, please see https://github.com/hobbyfarm/hobbyfarm/issues/236.

### `burst_count_capacity`

This field is a `map[string]int` designed to inform HobbyFarm how much capacity is available for dynamic provisioning in an environment. For dynamic provisioning to work, administrators need to set a count of the number of provisionable VM templates in this map. 

> This field may be removed in a future release of HobbyFarm. It is confusing to set the capacity of an environment in two locations. Further, removal of the concept of "bursting" from HobbyFarm may render this field useless. To track this discussion, please see https://github.com/hobbyfarm/hobbyfarm/issues/236

### `capacity_mode`

Possible values: `count` and `resource`

The capacity mode of an environment determines whether HobbyFarm schedules VMs based on how many VMs are already provisioned, or how much consumption there is of the basic resources (cpu, memory, storage). 

Practical usage almost always sets the value of this field to `count`. 

> This field may be removed in a future release of HobbyFarm. Real-world usage of HobbyFarm has shown that most users rely solely on the `count` strategy and do not utilize `resource`. To track this discussion, please see https://github.com/hobbyfarm/hobbyfarm/issues/237.