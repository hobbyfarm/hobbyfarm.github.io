+++
title = "Post-Install Setup"
weight = 3

+++

## Initial Admin User

The initial admin username is `admin` and password is `hobbyfarm`. 

> :warning: Make sure to change the password after your initial login.

## Initial VM Template

Before you can provision virtual machines, you must first define a `VirtualMachineTemplate`. A virtual machine template is a provider-agnostic representation of a template for a VM. It is used to schedule resources across different providers using a common definition. 

See [this link]({{< ref "/docs/architecture/resources/virtualmachinetemplate.md" >}}) for a sample VMT and more information on that resource.

## Seed Data
For other examples of resources see [architecture/resources]({{< ref "/docs/architecture/resources" >}})