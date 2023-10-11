+++
title = "Post-Install Setup"
weight = 4
+++

Now that HobbyFarm has been installed and an initial administrator account has been created, there are a few additional steps that must be performed before HobbyFarm can be used.

## Initial VM Template

Before you can provision virtual machines, you must first define a `VirtualMachineTemplate`. A virtual machine template is a provider-agnostic representation of a template for a VM. It is used to schedule resources across different providers using a common definition.

See [this link]({{< ref "/docs/architecture/resources/virtualmachinetemplate.md" >}}) for a sample VMT and more information on that resource.


## Configure Settings
HobbyFarm has some settings that can be adjusted via the Admin-UI or by altering the `setting` kubernetes objects directly.

