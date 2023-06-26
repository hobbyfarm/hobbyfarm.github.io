+++
title = "Helm Options"
description = "Reference document for the options available in the HobbyFarm Helm chart."
+++

This page contains a reference of the options available in the HobbyFarm Helm chart. 

## Admin UI Options

|Key|Default|Purpose|
|---|-------|-------|
|`admin.image`|`hobbyfarm/admin-ui:[version]`|Image used for admin-ui|
|`admin.configMapName`|`admin-config`|ConfigMap used to configure admin ui|
|`admin.config.title`|HobbyFarm Administration|Title of admin ui as seen in browser|
|`admin.config.favicon`| `/assets/default/favicon.png`|Path to the favicon to use for admin ui|
|`admin.config.login.logo`|`/assets/default/rancher-labs-stacked-color.svg`|Path to logo image as seen on admin ui login page|
|`admin.config.login.background`|`/assets/default/vault.jpg`|Path to large background image as seen on admin ui login page|
|`admin.config.logo`|`/assets/default/logo.svg`|Path to logo as seen in upper right corner of admin ui|

## UI Options

|Key|Default|Purpose|
|---|-------|-------|
|`ui.image`|`hobbyfarm/ui:[version]`|Image used for ui|
|`ui.configMapName`|`ui-config`|ConfigMap used to configure ui
|`ui.config.title`|HobbyFarm Learn|Title of ui as seen in browser|
|`ui.config.favicon`|`/assets/default/favicon.png`|Path to the favicon to use for ui|
|`ui.config.login.logo`|`/assets/default/rancher-labs-stacked-color.svg`|Path to logo image as seen on ui login page|
|`ui.config.login.background`|`/assets/default/login_container_farm.svg`|Path to large background image as seen on ui login page|
|`ui.config.logo`|`/assets/default/logo.svg`|Path to logo as seen in upper right corner of ui|
|`ui.about.title`|`About HobbyFarm`|Title of the about dropdown in ui|
|`ui.about.body`|`HobbyFarm is a browser-based technology training tool developed at github.com/hobbyfarm|

### `ui.config.about.buttons`

This list allows for specification of custom buttons in the About modal of the HobbyFarm UI. 

Example:
```yaml
ui:
  config:
    about:
      buttons:
        - title: Example Title
          url: https://github.com/hobbyfarm
        - title: Example Title 2
          url: https://github.com/hobbyfarm/hobbyfarm
```

### `ui.config.custom`

This heredoc allows for specification of custom CSS to be applied to the UI.

Example:
```yaml
ui:
  config:
    custom: |
      .custom-css {
        background: none;
      }
```

## Gargantua

|Key|Default|Purpose|
|---|-------|-------|
|`gargantua.image`|`hobbyfarm/gargantua:[version]`|Image used for Gargantua|
|`gargantua.logLevel`|`"0"`|Log level of Gargantua|
|`gargantua.dynamicBaseNamePrefix`|`"dynamic"`|Name prefix for VMs provisioned using dynamic method|
|`gargantua.scheduledBaseNamePrefix`|`"scheduled"`|Name prefix for VMs provisioned ahead of time (scheduled)|
|`gargantua.apiPort`|`80`|The port on which Gargantua's API should be served|
|`gargantua.webhook.containerPort`|`444`|Container port for CRD conversion webhook|
|`gargantua.webhook.servicePort`|`443`|Service port for CRD conversion webhook. :warning:|

 > :warning: This port should remain 443. See [this link](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definition-versioning/#deploy-the-conversion-webhook-service) for more information.

## Guacamole

|Key|Default|Purpose|
|---|-------|-------|
|`guac.image`|undefined|Image used when deploying guac server for Windows RDP|

## Shell

|Key|Default|Purpose|
|---|-------|-------|
|`shell.replicas`|`1`|Number of replicas of the shell service to run|

## Ingress

|Key|Default|Purpose|
|---|-------|-------|
|`ingress.enabled`|`false`|Enable ingress
|`ingress.annotations`|`{}`|Additional annotations for ingress|
|`ingress.tls.enabled`|`true`|Enable TLS on ingress|
|`ingress.tls.secrets.backend`|`backend-tls-secret`|TLS secret for backend ingress|
|`ingress.tls.secrets.admin`|`admin-tls-secret`|TLS secret for admin ui ingress|
|`ingress.tls.secrets.ui`|`ui-tls-secret`|TLS secret for ui ingress|

## Terraform


> :warning::warning::warning: Terraform will be removed in a future version. It is NOT recommended to use Terraform as there are compatibility, instability and performance issues. 

The options are not documented here as it is strongly inadvisable to use Terraform. Please instead use a provider-specific operator for VM provisioning. 

## RancherVM

> :warning::warning::warning: RancherVM is old, old, old, and seriously deprecated. Do not use this. It will be removed in a future version. 