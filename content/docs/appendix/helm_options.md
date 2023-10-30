+++
title = "Helm Options"
description = "Reference document for the options available in the HobbyFarm Helm chart."
+++

This page contains a reference of the options available in the HobbyFarm Helm chart. Please [visit the values.yaml](https://github.com/hobbyfarm/hobbyfarm/blob/master/charts/hobbyfarm/values.yaml) file in the HobbyFarm repository for the most up-to-date list of options.

## General

| Key | Default | Purpose |
| --- | ------- | ------- |
| `insecure` | `false` | Use insecure protocols. Useful for local development. |

## Admin UI Options

| Key | Default | Purpose |
| --- | ------- | ------- |
| `admin.image` | `hobbyfarm/admin-ui:[version]` | Image used for admin-ui. |
| `admin.configMapName` | `admin-config` | ConfigMap used to configure admin ui. |
| `admin.config.title` | HobbyFarm Administration | Title of admin ui as seen in browser. |
| `admin.config.favicon` | `/assets/default/favicon.png` | Path to the favicon to use for admin ui. |
| `admin.config.login.logo` | `/assets/default/rancher-labs-stacked-color.svg` | Path to logo image as seen on admin ui login page. |
| `admin.config.login.customlogo` | undefined | A Secret containing an SVG as a file. |
| `admin.config.login.background` | `/assets/default/vault.jpg` | Path to large background image as seen on admin ui login page. |
| `admin.config.logo` | `/assets/default/logo.svg` | Path to logo as seen in upper right corner of admin ui. |

## UI Options

| Key | Default | Purpose |
| --- | ------- | ------- |
| `ui.image` | `hobbyfarm/ui:[version]` | Image used for ui. |
| `ui.configMapName` | `ui-config` | ConfigMap used to configure ui. |
| `ui.config.title`| `HobbyFarm Learn` | Title of ui as seen in browser. |
| `ui.config.favicon` | `/assets/default/favicon.png` | Path to the favicon to use for ui. |
| `ui.config.login.logo` | `/assets/default/rancher-labs-stacked-color.svg` | Path to logo image as seen on ui login page. |
| `ui.config.login.background` | `/assets/default/login_container_farm.svg` | Path to large background image as seen on ui login page. |
| `ui.config.logo` | `/assets/default/logo.svg` | Path to logo as seen in upper right corner of ui. |
| `ui.about.title` | `About HobbyFarm` | Title of the about dropdown in ui. |
| `ui.about.body` | `HobbyFarm is a browser-based technology training tool developed at github.com/hobbyfarm` | |
| `ui.config.about.buttons` | undefined | See the [button section](#uiconfigaboutbuttons) below for more information. |
| `ui.config.custom` | undefined | See the [custom section](#uiconfigcustom) below for more information. |

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

| Key | Default | Purpose |
| --- | ------- | ------- |
| `gargantua.image` | `hobbyfarm/gargantua:[version]` | Image used for the Gargantua backend. |
| `gargantua.logLevel` | `"0"` | Log level of Gargantua. |
| `gargantua.dynamicBaseNamePrefix` | `"dynamic"` | Name prefix for VMs provisioned using dynamic method. |
| `gargantua.scheduledBaseNamePrefix` | `"scheduled"` | Name prefix for VMs provisioned ahead of time (scheduled). |
| `gargantua.apiPort` | `80` | The port on which Gargantua's API should be served. |
| `gargantua.webhook.containerPort` | `444` | Container port for CRD conversion webhook. |
| `gargantua.webhook.servicePort` | `443` | Service port for CRD conversion webhook. :warning: |

 > :warning::warning: The `gargantua.webhook.servicePort` port should remain 443. See [this link](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definition-versioning/#deploy-the-conversion-webhook-service) for more information. :warning::warning:

## Guacamole

| Key | Default | Purpose |
| --- | ------- | ------- |
| `guac.image` | undefined | Image used when deploying [Guacamole server](https://guacamole.apache.org) for Windows RDP. |

## Shell

| Key | Default | Purpose |
| --- | ------- | ------- |
| `shell.replicas` | `1` | Number of replicas of the shell service to run. |

## Ingress

| Key | Default | Purpose |
| --- | ------- | ------- |
| `ingress.enabled` | `false` | Enable ingress. |
| `ingress.annotations` | `{}` | Additional annotations for ingress. |
| `ingress.tls.enabled` | `true` | Enable TLS on ingress. |
| `ingress.tls.secrets.backend` | undefined | TLS secret for backend ingress. |
| `ingress.tls.secrets.admin` | undefined | TLS secret for admin ui ingress. |
| `ingress.tls.secrets.ui` | undefined | TLS secret for ui ingress. |
| `ingress.tls.secrets.shell` | undefined | TLS secret for shell ingress. |
| `ingress.hostnames.ui` | undefined | Hostname for ui ingress. |
| `ingress.hostnames.admin` | undefined | Hostname for admin ui ingress. |
| `ingress.hostnames.backend` | undefined | Hostname for backend ingress. |
| `ingress.hostnames.shell` | undefined | Hostname for shell ingress. |

## Users

| Key | Default | Purpose |
| --- | ------- | ------- |
| `users.admin.enabled` | `false` | Creates the user, `admin`. |
| `users.admin.password` | `$2a$10$9ToKMT37Z7K70xbCVnW9KOzEuPq0JyW/rA06gsukD4U9YfKFjMXTe` | Bcrypt hashed password for the `admin` user. Default value is `Sup3r@dmin`. |

### Adding Users
Additional users can be added by adding a new entry to the `users` list. The following example shows how to add a user named `user1` with the password `password1`.

```yaml
users:
  admin:
    enabled: true
    password: $2a$10$9ToKMT37Z7K70xbCVnW9KOzEuPq0JyW/rA06gsukD4U9YfKFjMXTe
  user1:
    enabled: true
    password: $2a$12$DNBNq4TZarIdx.IaGjcdSOjKhubC0A4CpD5asd7dajgGJwjPROIlu
```

## Terraform

> :warning::warning::warning: Terraform will be removed in a future version. It is NOT recommended to use Terraform as there are compatibility, instability and performance issues.

The options are not documented here as it is strongly inadvisable to use Terraform. Please instead use a [provider-specific operator](/docs/configuration/provisioners) for VM provisioning.