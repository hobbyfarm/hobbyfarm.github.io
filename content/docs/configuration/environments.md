+++
title = "Environments"
description = "Configure the environments used by HobbyFarm for deploying virtual machines."
weight = 3
+++

An `Environment` defines specific implementation details on how HobbyFarm connects to a provider, such as DigitalOcean or AWS, for scheduling virtual machines. An environment defines configuration information such as provider credentials, virtual machine templates to use, and the total capacity of virtual machines which can be deployed.

> **NOTE:** At least one [VM Template](/docs/configuration/vmtemplates) **_must_** be created before creating an `Environment`.

> **NOTE:** An environment must be configured with a provisioner before a virtual machine can be created by HobbyFarm.

## Creating an Environment

To create a new `Environment`, click on the `+NEW` button in the top left corner of the page under the `Environments` heading. A popup will appear with a form to fill out the details of the new `Environment`. The following information will explain each field in the form.

### Basic Information

The `Basic Information` section of the form is used to provide basic information about the `Environment`.

![Environment - Basic Information](/images/hobbyfarm-admin-environment-basic.png)

#### Variables

| Setting | Description |
| --- | --- |
| **_Display Name_** | The name of the `Environment`. This name will be used to identify the environment in the HobbyFarm Admin-UI and API. |
| **_DNS Suffix_** | **\*\*DEPRECATED\*\*** This feature will be removed in a future release. |
| **_Provider_** | The provider to use for the environment, ie. `digitalocean`, `aws`, etc. |
| **_Websocket Endpoint_** | This should be the URL for the `shell` domain, ie. `shell.{domain}.com`. |

### Environment Specifics

### VM Templates

### IP Mappings

### Confirmation