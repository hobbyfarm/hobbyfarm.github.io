+++
title = "Provisioners"
description = "Configure and manage provisioners used by HobbyFarm environments."
weight = 4
+++

In HobbyFarm, a provisioner is a Kubernetes resource used to define the specific implementation details on how HobbyFarm connects to a provider, such as DigitalOcean or AWS, for scheduling virtual machines. A provisioner defines configuration information such as provider credentials, virtual machine templates to use, and the total capacity of virtual machines which can be deployed.

## Current Provisioners

Below is a list of the provisioners currently supported by HobbyFarm. Please visit the links below for more information on each provisioner, including documentation on how to deploy and configure the provisioners.

| Provisioner | Provider | Current Version | Description |
| --- | --- | --- | --- |
| [hf-provisioner-digitalocean](https://github.com/hobbyfarm/hf-provisioner-digitalocean) | DigitalOcean | v0.0.1 | A provisioner for DigitalOcean. |
| [hf-provisioner-ec2](https://github.com/hobbyfarm/hf-provisioner-ec2) | AWS | WIP | A provisioner for AWS. |

> **NOTE:** At least one [Environment](/docs/configuration/environments) **_must_** be created before a provisioner can be used in HobbyFarm.

## Upcoming Provisioners
The HobbyFarm development team is working on a new provisioning method which will be released in a future version of HobbyFarm. These new provisioners will be easier to use and will provide a more consistent experience in the HobbyFarm Admin-UI.

## Terraform Provisioning
:warning::warning::warning: **DEPRECATED** :warning::warning::warning:

Older versions of HobbyFarm used Terraform as the means of provisioning virtual machines in cloud environments. However, this feature was deprecated and is no longer supported due to compatibility, stability, and performance issues.