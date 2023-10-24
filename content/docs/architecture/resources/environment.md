+++
title = "Environment"
description = "Defines specific implementation details on how HobbyFarm connects to a provider for scheduling virtual machines."
+++


An `Environment` resource contains configuration information such as where provider credentials are stored, what image to use when creating a VM, or how much capacity a provider has for a specific type of VM.

## Kubernetes Commands
The following commands are useful for managing Environment resources in Kubernetes.

```bash
## Get a list of all Environments
kubectl get environments -n hobbyfarm-system

## Create an Environment from a YAML manifest
kubectl apply -f {environmentManifest} -n hobbyfarm-system

## Edit an Environment
kubectl edit environment {environmentName} -n hobbyfarm-system

## Backup an Environment to a YAML manifest
kubectl get environment {environmentName} -n hobbyfarm-system -o yaml > {environmentManifest}

## Delete an Environment
kubectl delete environment {environmentName} -n hobbyfarm-system
```


## Environment Manifest Example
The following shows an example of an Environment manifest in Kubernetes.

```yaml
apiVersion: hobbyfarm.io/v1
kind: Environment
metadata:
  annotations:
    hobbyfarm.io/provisioner: external
  name: demo-environment
  namespace: hobbyfarm-system
spec:
  count_capacity:
    example-do-vm-template: 10
  display_name: demo-environment
  environment_specifics:
    region: nyc1
    token-secret: do-secret
  provider: digitalocean
  template_mapping:
    example-do-vm-template:
      image: ubuntu-20-04-x64
      size: s-1vcpu-512mb-10gb
      ssh_username: root
  ws_endpoint: shell.{domain}.com
```

> **NOTE:** The inclusion of the `hobbyfarm.io/provisioner: external` annotation is required when using an external provisioner. However, the use of this annotation **_will_** break provisioning with Terraform. See the [Provisioner documentation](/docs/configuration/provisioners) for more information.

## Configuration

### `count_capacity`
This field is a `map[string]int` and is designed to inform HobbyFarm about the capacity of the environment for a particular VM template. Each key in this map is the name of the VM template, and the value is the number of total VMs of that type that the environment can support.

This map should be set according to what resources an environment has. Take care to account for multiple templates and totals that will affect the amount of instances, cpu, memory, etc. that are consumed in the provider.

> **NOTE:** In the example manifest shown above, the environment can support 10 total VMs of the `example-do-vm-template` type.

### `display_name`
Display name for the environment.

### `environment_specifics`
This field is a `map[string]string` designed to inform HobbyFarm about specific values for the environment. Entries in this map are utilized by VM provisioners for purposes.

The keys defined here are specific to the provisioner in use for the environment, such as DigitalOcean or AWS. Each provisioner may have specific values that are required.

> **NOTE:** In the example manifest shown above, the DigitalOcean provisioner requires a `region` and `token-secret` to be defined.

### `provider`
This field is used to determine what provisioner should be utilized for an environment and **_must_** be used in conjunction with the `hobbyfarm.io/provisioner: external` annotation to set the external provisioner. See the [example manifest above](#environment-manifest-example) for a demonstration of this annotation.

> **NOTE:** Current available options are `digitalocean` and `aws`.

> **NOTE:** An external provider must be installed separately from HobbyFarm. See the [Provisioner documentation](/docs/configuration/provisioners) for more information.

#### Terraform Deprecation
The logic behind the `provider` field has changed. It was originally intended for use with Terraform for VM provisioning. The field originally informed HobbyFarm what Terraform module to use when provisioning. Since [Terraform is deprecated](/docs/configuration/provisioners/#terraform-provisioning), this field is now used to inform HobbyFarm what external provisioner to use for an environment.

### `template_mapping`

This field is a `map[string]string` allowing administrators to configure specific values for each VM template in use in the environment. Some common uses of this map are for elements such as instance size or AMI. 

The keys defined here are specific to the provisioner in use for the environment. Each provisioner may have specific values that are required. 

### `ws_endpoint`

This field defines which websocket (shell) endpoint should be used by end-user clients to open a shell to VMs provisioned in this environment. This field may be used in situations where a specific shell server needs to be utilized. An example of this may be an environment where the shell server needs to be in a DMZ in order to access VMs that are otherwise isolated from the main HobbyFarm install. 