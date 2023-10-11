+++
title = "VM Templates"
description = "Configure templates for virtual machines used by HobbyFarm."
weight = 2
+++

The VM Templates page allows administrators to configure templates for virtual machines used by HobbyFarm. These templates are used in by HobbyFarm `Environments` to create virtual machines on 3rd party platforms. To access the Settings page, log into the HobbyFarm Admin-UI and click the `Configuration` option in the top navigation menu. In the left navigation menu, click the `VM Templates` option.

## Creating a VM Template

To create a new VM Template, click on the `+NEW` button in the top left corner of the page under the `VM Templates` heading. A popup will appear with a form to fill out the details of the new VM Template. The following information will explain each field in the form.

### Basic Information

The `Basic Information` section of the form is used to provide basic information about the VM Template.

![VM Template - Basic Information](/images/hobbyfarm-admin-vmtemplate-basic.png)

#### Variables

| Setting | Description |
| --- | --- |
| **_Name_** | The name of the VM Template. This name will be used to identify the VM Template in the HobbyFarm Admin-UI and API. |
| **_Image_** | The image to use for the VM Template. The image must be a valid image existing on the provider platform. |


#### Image Examples
| Provider | Image | Image Type | AMI URL |
| --- | --- | --- | --- |
| DigitalOcean | ubuntu-20-04-x64 | Ubuntu 20.04 LTS x64 | [DO Available Images](https://do-community.github.io/available-images/) |
| AWS | ami-0a6e38961e6e621b0 | Ubuntu 20.04 LTS x64 | [Find a Linux AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html) |

### Config Map

### Services

### Confirmation