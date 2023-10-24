+++
title = "Provider Images"
description = "Information about the provider images used by HobbyFarm."
+++

A provider image is a virtual machine image which exists on a 3rd party cloud provider, such as [DigitalOcean](https://www.digitalocean.com/) or [AWS](https://aws.amazon.com/). These images are used by HobbyFarm to deploy virtual machines per cloud environment. The provider image is used to determine the operating system, software, and configuration of the virtual machine deployed and managed by HobbyFarm.

## Image Examples
| Provider | Image | Image Type | AMI URL |
| --- | --- | --- | --- |
| DigitalOcean | ubuntu-20-04-x64 | Ubuntu 20.04 LTS x64 | [DO Available Images](https://do-community.github.io/available-images/) |
| AWS | ami-0a6e38961e6e621b0 | Ubuntu 20.04 LTS x64 | [Find a Linux AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html) |

## Custom Images
HobbyFarm supports the use of custom cloud images, for example a [custom built AWS AMI](https://developer.hashicorp.com/packer/integrations/hashicorp/amazon) built using [Hashicorp's Packer tool](https://www.packer.io). To use a custom image, the image must be built, pushed to the respective cloud platform, and accessible by [the provisioner](/docs/configuration/provisioners) used by HobbyFarm.

> **NOTE:** Container images are not supported at this time.