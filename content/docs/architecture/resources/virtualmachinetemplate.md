+++
title = "Virtual Machine Template"
description = "A provider-agnostic representation of a template for a virtual machine."
+++

A `VirtualMachineTemplate` allows the HobbyFarm platform to schedule resources across multiple providers while providing a common definition for content creators. It is a required resource for creating an [Environment](/docs/architecture/resources/environment).

A VM template represents a potential VM configuration, with its implementation varying by provider. For instance, [AWS](https://aws.amazon.com) might interpret the template as a `t3.medium`, while [Azure](https://azure.microsoft.com) might implement it as a `Standard_D2_v2`.

Each [Environment](/docs/architecture/resources/environment) is responsible for determining the specific realization of a `VirtualMachineTemplate`.


## Kubernetes Commands
The following commands are useful for managing VirtualMachineTemplate resources in Kubernetes.

```bash
## Get a list of all VirtualMachineTemplates
kubectl get virtualmachinetemplates -n hobbyfarm-system

## Create a VirtualMachineTemplate from a YAML manifest
kubectl apply -f {virtualMachineTemplateManifest} -n hobbyfarm-system

## Edit a VirtualMachineTemplate
kubectl edit virtualmachinetemplate {virtualMachineTemplateName} -n hobbyfarm-system

## Backup a VirtualMachineTemplate to a YAML manifest
kubectl get virtualmachinetemplate {virtualMachineTemplateName} -n hobbyfarm-system -o yaml > {virtualMachineTemplateManifest}

## Delete a VirtualMachineTemplate
kubectl delete virtualmachinetemplate {virtualMachineTemplateName} -n hobbyfarm-system
```

## Example VirtualMachineTemplate Manifest
The following shows an example of a VirtualMachineTemplate manifest in Kubernetes.

```yaml
apiVersion: hobbyfarm.io/v1
kind: VirtualMachineTemplate
metadata:
  name: vmt-neuc2clbtv
  namespace: hobbyfarm-system
spec:
  config_map:
    cloud-config: ""
    image: ubuntu-20-04-x64
    size: s-1vcpu-512mb-10gb
    ssh_username: root
    webinterfaces: '[]'
  image: ubuntu-20-04-x64
  name: do-ubuntu-20-04-x64
```

## Configuration
### `name`
Display name for the VM template.

### `image`
Default value for the image to use when provisioning this VM.

Acceptable values will depend upon the provisioner in use. Useful either as a default value for a default provisioner, or when multiple environments use the same provisioner. Can be overridden via configuration in the `Environment` resource.

### `config_map`
The ConfigMap provides the ability to customize more values given to the provisioner. Useful if the provisioner can distinguish between regions or different sizing options.

An example for a `config_map` for virtual machines on [Hetzner](https://www.hetzner.com) could look the following 

```yaml
## Example Hetzner config_map
spec:
  config_map:
    server_type: cx21   ## Hetzner has available sizings cx11, cx21, cx31 ...
    ssh_username: root  ## Default SSH Username is root
    location: nbg1      ## Region
```

## Cloud-Init configuration
`VirtualMachineTemplates` may offer [Cloud-Init](https://cloudinit.readthedocs.io/en/latest/) configuration. Currently this is saved under the `cloud-config` key inside the `config_map`.

```yaml
## Example Cloud-Init configuration
spec:
  config_map:
    cloud_config: |
      runcmd:
        - echo "Hello World"
```

> **NOTE:** Future releases of HobbyFarm will allow the Cloud-Init configuration to be saved in it's own field inside the VirtualMachineTemplate.

## Services and Webinterfaces
Services and webinterfaces can be configured for VMTs. A Service may provide a webinterface on a specific port and path which will be shown to the user in an iFrame. This webinterface will be proxied to ensure authorization.
Currently this should be configured through the UI as it is stored in an array under the `webinterfaces` key inside the `config_map`.
You can define presets of often used services using [PredefinedServices]({{< ref "/docs/architecture/resources/predefinedservice" >}}).