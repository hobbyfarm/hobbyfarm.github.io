+++
title = "Deployment"
description = "Steps for the deployment of HobbyFarm via Helm."
weight = 2
+++

The following steps will guide you through the deployment of HobbyFarm in a Kubernetes environment using Helm. Please ensure that all [prerequisites](/docs/setup/prerequisites) are met before proceeding with the installation. Failure to do so may result in an unsuccessful installation.

## Step 1: Add Helm Repository

Using Helm, add the HobbyFarm repository and generate a values.yaml file for customization.

```bash
## Add the required Helm repository
helm repo add hobbyfarm https://hobbyfarm.github.io/hobbyfarm

## Update your local Helm repository cache
helm repo update

## List available versions of HobbyFarm
helm search repo hobbyfarm/hobbyfarm

## Get the values.yaml for HobbyFarm
helm show values hobbyfarm/hobbyfarm > values.yaml
```

## Step 2: Customize the Values File

There are several options available in the `values.yaml` file. Please refer to the [Helm options](/docs/appendix/helm_options) documentation for a full reference of these values. Additionally, the [HobbyFarm Helm Chart values.yaml](https://github.com/hobbyfarm/hobbyfarm/blob/master/charts/hobbyfarm/values.yaml) file is available for review and contains additional comments and examples.

### `Hostname Configuration`

HobbyFarm requires four domains to serve its traffic. Prior to installation of HobbyFarm, these domains **_must exist_** in a DNS server and must point to either a load balancer or a Kubernetes Node. Documentation of DNS configuration is outside the scope of this documentation.

| Domain | Purpose | HobbyFarm Service |
|----------------|---------|---------|
| api.{domain}.com | API & Controller Hosting | Gargantua Backend |
| admin.{domain}.com | Administrative Interface | Admin-UI Frontend |
| learn.{domain}.com | End-User Learning Interface | UI Frontend |
| shell.{domain}.com | Shell Endpoint for UI | Gargantua Shell |

> **NOTE:** The documentation assumes the above domain prefixes are used for deployment of HobbyFarm. It is **_not required_** to adhere to the prefixes shown in the examples above (e.g. api, admin). Users may choose any desired prefix.

> :warning: HobbyFarm may work using sub-paths (e.g. {domain}.com/api) but this is not currently a tested configuration.

### `TLS Configuration`
HobbyFarm uses [Kubernetes Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) for Web traffic and routing. If TLS is enabled, HobbyFarm will make use of certificates referenced via the Ingress resource. [Kubernetes TLS Secrets](https://kubernetes.io/docs/concepts/configuration/secret/#tls-secrets) can be created containing the key and certificate for each domain, and referenced in the `values.yaml` file.

Example Kubernetes Secrets TLS manifest:
```yaml
## Create a Secret containing the TLS certificate and key for the backend domain, api.{domain}.com
apiVersion: v1
kind: Secret
metadata:
  name: backend-tls-secret
  namespace: hobbyfarm-system    ## The Secret must be created in the HobbyFarm namespace
type: kubernetes.io/tls
data:
  tls.crt: <base64 encoded certificate>  ## Replace with a valid base64 encoded certificate
  tls.key: <base64 encoded key>          ## Replace with a valid base64 encoded key
```

> **NOTE:** The creation of certificates is outside the scope of this documentation. Cert-manager can be used to automate the creation of TLS certificates. Please refer to the [cert-manager documentation](https://cert-manager.io/docs/) for more information.

The following example shows the configuration for TLS in the `values.yaml` file:
```yaml
ingress:
  enabled: true
  tls:
    enabled: true
    secrets:
      backend:    backend-tls-secret  ## References the Secret created above
      admin:      admin-tls-secret
      ui:         ui-tls-secret
      shell:      shell-tls-secret
    hostnames:
      backend:    api.{domain}.com    ## Hostname for the backend, used in the above Secret
      admin:      admin.{domain}.com
      shell:      shell.{domain}.com
      ui:         learn.{domain}.com
```

If unique certificates per hostname are required, a unique Secret per hostname must be created and associated with the hostname in the `values.yaml` file. Ensure each certificate stored in a Secret corresponds to the correct hostname, otherwise traffic will not be routed correctly.

> **NOTE:** Wildcard certificates **_are_** acceptable.


## Step 3: Create a Namespace

Create a namespace to install HobbyFarm. For example:

```bash
kubectl create namespace hobbyfarm-system
```

> **NOTE:** The namespace name is arbitrary and can be changed to suit your needs. However, be sure to use the same namespace name throughout the installation process.

## Step 4: Install HobbyFarm via Helm

Install HobbyFarm using a values file that you have created, or by specifying your chosen options using Helm's `--set` flags.

```bash
## Install HobbyFarm using a values file
helm upgrade \
  --install hobbyfarm hobbyfarm/hobbyfarm \
  --namespace hobbyfarm-system \
  --values values.yaml

## Install HobbyFarm using --set flags
helm upgrade \
  --install hobbyfarm hobbyfarm/hobbyfarm \
  --namespace hobbyfarm-system \
  --set ingress.tls.enabled=true
```

> **NOTE:** Using the `upgrade --install` command will install HobbyFarm if it does not exist, or upgrade it if it does exist.

## Step 5: Verify Installation

Verify that HobbyFarm has been installed successfully by checking the status of the HobbyFarm pods.

```bash
## Use the watch command to monitor the status of the HobbyFarm pods
watch -n1 kubectl get pods -n hobbyfarm-system

## Use the watch commadn to monitor the deployments until all are in a Ready state
watch -n1 kubectl get deployment -n hobbyfarm-system
```

## Step 6: Setup an Admin User

Once installation has been verified, proceed to the [Admin Account](/docs/setup/initial_admin) documentation to create an initial administrator account.
