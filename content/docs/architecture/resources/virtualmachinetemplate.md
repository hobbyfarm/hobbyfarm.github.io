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
    image: ubuntu-2204 # general reference, not specific identifier
    resources:
        cpu: 2
        memory: 4096
        storage: 10
``` 


A VM template is merely a representation of a possible VM implementation by a provider. For example, in AWS this template may correspond to the creation of a `t3.medium` whereas in Azure it may correspond to `Standard_D2_v2`. It is up to each `Environment` to determine how to implement a `VirtualMachineTemplate`.

