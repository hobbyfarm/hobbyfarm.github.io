+++
title = "VM Templates"
description = "Configure templates for virtual machines used by HobbyFarm."
weight = 2
+++

The VM Templates page allows administrators to configure templates for virtual machines used by HobbyFarm. These templates are used in by HobbyFarm `Environments` to create virtual machines on 3rd party platforms.

To access the Settings page, log into the HobbyFarm Admin-UI and click the `Configuration` option in the top navigation menu. In the left navigation menu, click the `VM Templates` option.

## Creating a VM Template

To create a new VM Template, click on the `+NEW` button in the top left corner of the page under the `VM Templates` heading. A popup will appear with a form to fill out the details of the new VM Template. The following information will explain each field in the form.

### `Basic Information`

The `Basic Information` section of the form is used to provide basic information about the VM Template.

![VM Template - Basic Information](/images/hobbyfarm-admin-vmtemplate-basic.png)

#### Variables

| Setting | Description |
| --- | --- |
| **_Name_** | The name of the VM Template. This name will be used to identify the VM Template in the HobbyFarm Admin-UI and API. |
| **_Image_** | The image to use for the VM Template. The image must be a valid image existing on the provider platform. |

> **NOTE:** Please visit the [Provider Images](/docs/appendix/provider_images) page for more information on provider images and how to find an image for a specific provider.

### `Config Map`

The `Config Map` section of the form is used to provide configuration information for the VM Template. Variables are passed in a key/value format, where the key is the name of the variable and the value is the value of the variable.

![VM Template - Config Map](/images/hobbyfarm-admin-vmtemplate-configmap.png)

> **NOTE:** Please visit the [Virtual Machine Template](/docs/architecture/resources/virtualmachinetemplate) Resource documentation for more information on `Config Map` variables.

### `Services`

The `Services` section of the form defines services running on the virtual machine which are available for use by the end-user.

For instance, a virtual machine can run a service like [Visual Studio Code Server](https://code.visualstudio.com/docs/remote/vscode-server), which would allow a user to securely connect to the virtual machine via a local VS Code client. This running service can be exposed to the Internet via a specific port number, ie. 8080, on the virtual machine.

When a service is made available using the VM Template, it will be presented to the user in a tab next to VM shell in the HobbyFarm User-UI. This makes it incredibly easy for users to find and use the service.

![VM Template - Services](/images/hobbyfarm-admin-vmtemplate-services.png)

### `Confirmation`

Finally, the `Confirmation` section of the form is used to confirm the details of the VM Template before it is created. Once the details have been verified, click the `FINISH` button in the lower right corner to create the VM Template.

![VM Template - Confirmation](/images/hobbyfarm-admin-vmtemplate-confirmation.png)

## Next Steps

Once a VM Template has been created, it can be used in an `Environment` to create virtual machines. Please visit the [Environments](/docs/configuration/environments) page for more information on creating an environment.