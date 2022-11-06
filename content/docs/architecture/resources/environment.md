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

### `count_capacity`

This field is a `map[string]int` designed to inform HobbyFarm about the capacity of the environment for a particular VM template. Each key in this map is the name of the VM template, and the value is the number of total VMs of that type that the environment can support. 

For example:
```yaml
...
count_capacity:
    example-template: 100
```

The above example indicates that at any given point in time, the environment can support up to 100 total `example-template` VMs.

This map should be set according to what resources an environment has. Take care to account for multiple templates and totals that will affect the amount of instances, cpu, memory, etc. that are consumed in the provider. 

### `display_name`

Display name for the environment.

### `environment_specifics`

This field is a `map[string]string` designed to inform HobbyFarm about specific values for the environment. Entries in this map are utilized by VM provisioners for purposes such as security group ID, VPC, etc. The keys defined here are specific to the provisioner in use for the environment. Each provisioner may have specific values that are required.

### `provider`

This field is used to determine what provisioner should be utilized for an environment. 

The logic behind this field is somewhat complicated due to the original use of only Terraform for VM provisioning. The field originally informed HobbyFarm what Terraform module to use when provisioning. 

With the advent of external provisioners this field now determines what external provisioner is used. When used in combination with the annotation `hobbyfarm.io/provisioner: external`, a matching external provisioner (to the `provider` field) will be used. 

### `template_mapping`

This field is a `map[string]string` allowing administrators to configure specific values for each VM template in use in the environment. Some common uses of this map are for elements such as instance size or AMI. 

The keys defined here are specific to the provisioner in use for the environment. Each provisioner may have specific values that are required. 

### `ws_endpoint`

This field defines which websocket (shell) endpoint should be used by end-user clients to open a shell to VMs provisioned in this environment. This field may be used in situations where a specific shell server needs to be utilized. An example of this may be an environment where the shell server needs to be in a DMZ in order to access VMs that are otherwise isolated from the main HobbyFarm install. 