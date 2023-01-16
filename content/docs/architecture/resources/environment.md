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