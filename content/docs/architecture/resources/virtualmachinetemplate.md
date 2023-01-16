+++
title = "Virtual Machine Template"
+++

A `VirtualMachineTemplate` is a provider-agnostic representation of a template for a VM. It allows HobbyFarm to schedule resources across multiple providers while providing a common definition for content creators. 

Here's a sample virtual machine manifest:

```yaml
apiVersion: hobbyfarm.io/v1
kind: VirtualMachineTemplate
metadata:
    name: example-vmt
    namespace: hobbyfarm-system
spec:
    id: example-vmt
    name: Example VMTemplate
    image: ami-08921390234098
    config_map: {"key": "value", "another_key": ... [] ...}
``` 


A VM template is merely a representation of a possible VM implementation by a provider. For example, in AWS this template may correspond to the creation of a `t3.medium` whereas in Azure it may correspond to `Standard_D2_v2`. It is up to each `Environment` to determine how to implement a `VirtualMachineTemplate`.

## Configuration

### `id`

Identifier for the VM template. Should be identical to the Kubernetes `metadata.name` field. Present for historical reasons, will be phased out in a future release. 

> This field is currently *required* in HobbyFarm. Architecturally it is considered deprecated and may be removed in a future release. For now, users must continue to set this field. 

### `name`

Display name for the VM template.

### `image`

Default value for the image to use when provisioning this VM. Acceptable values will depend upon the provisioner in use. Useful either as a default value for a default provisioner, or when multiple environments use the same provisioner. Can be overridden via configuration in the `Environment` resource. 

### `config_map`
The ConfigMap provides the ability to customize more values given to the provisioner. Useful if the provisioner can distinguish between Regions or different sizing options. 

An example for a configmap vor VMs on Hetzner could look the following 

```yaml
server_type:   cx21 # Hetzner has available sizings cx11, cx21, cx31 ...
ssh_username:  root # Default SSH Username is root
location:      nbg1 # Region
```
