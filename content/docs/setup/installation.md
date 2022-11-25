+++
title = "Installation"
weight = 2

+++

## 1 - Add Helm Repository

Add the required Helm repository:

`helm repo add hobbyfarm https://hobbyfarm.github.io/hobbyfarm`

## 2 - Examine hostnames and prepare Certificates

HobbyFarm requires four domains to serve its traffic:

|Domain Example|Purpose|
|--------------|--------|
|api.your-hobbyfarm.com|API & Controller Hosting|
|admin.your-hobbyfarm.com|Administrative Interface|
|learn.your-hobbyfarm.com|End-User Learning Interface|
|shell.your-hobbyfarm.com|WebSocket Shell Endpoint|

Prior to installation of HobbyFarm you will need to create these entries in your DNS server(s) and point them to your load balancer, ingress endpoint, etc. Documentation of that process is outside the scope of this document. 

> You are not required to adhere to these specifix prefixes (e.g. api, admin). You may choose your own. 

> :warning: While it *may* work to host HobbyFarm at multiple sub-paths (e.g. your-hobbyfarm.io/learn) this is not currently a tested configuration. 

HobbyFarm uses ingress for its web traffic and if TLS is enabled it will make use of certificates referenced via the ingress resource. You can specify the secret(s) that contains the key and certificate via the following `values.yaml` keys:

```yaml
tls:
    enabled: true
    secrets:
        backend:    backend-tls-secret  # replace with your own
        admin:      admin-tls-secret    # can be the same for each
        ui:         ui-tls-secret       # e.g. if you are using *.your-hobbyfarm.com
    hostnames:
        backend:    api.your-hobbyfarm.com
        admin:      admin.your-hobbyfarm.com
        shell:      shell.your-hobbyfarm.com
        ui:         learn.your-hobbyfarm.com
```

Please ensure that each certificate corresponds to the correct hostname as specified in `values.yaml`. For example, if you define a secret (as named in the example above) of `backend-tls-secret`, make sure the certificate in this secret is valid for `api.your-hobbyfarm.com`. Wildcard certificates *are* acceptable.

## 2 - Customize Values File

There are several options available in the `values.yaml` file for this helm chart. Please refer to the [helm options](appendix/helm_options.md) document for a full reference of these values. 

## 3 - Create Namespace

Create a namespace of your choice in which to install HobbyFarm. For example:

```bash
kubectl create namespace hobbyfarm-system
```

## 4 - Install HobbyFarm 

Install HobbyFarm using a values file that you have created, or by specifying your chosen options using Helm's `--set` flags.

For example:

```bash
helm install hobbyfarm hobbyfarm/hobbyfarm --namespace hobbyfarm-system --set ingress.tls.enabled=true
```

## 5 - Complete

Installation should be complete at this point. Please proceed to [post-install setup](setup/post_install).