+++
title = "Environments"
description = "Configure the environments used by HobbyFarm for deploying virtual machines."
weight = 3
+++

An `Environment` defines specific implementation details on how HobbyFarm connects to a provider, such as DigitalOcean or AWS, for scheduling virtual machines. An environment defines configuration information such as provider credentials, virtual machine templates to use, and the total capacity of virtual machines which can be deployed.

> **NOTE:** At least one [VM Template](/docs/configuration/vmtemplates) **_must_** be created before creating an `Environment`.

> **NOTE:** An environment must be configured with a provisioner before a virtual machine can be created by HobbyFarm.