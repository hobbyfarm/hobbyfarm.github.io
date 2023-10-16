+++
title = "PredefinedService"
description = "Configure pre-defined services with VirtualMachineTemplates."
+++

A `PredefinedService` provides pre-configured Services that are available when creating or updating [VirtualMachineTemplates](/docs/architecture/resources/virtualmachinetemplate). The services can be configured to provide a web interface that is proxied to the user.

## Example PredefinedService Manifest
The following shows an example of a PredefinedService manifest in Kubernetes. This example shows the installation of [code-server](https://github.com/coder/code-server), which provides the ability to run VS Code on any machine and access the instance via a Web browser. Additional manifest examples can be found at the bottom of this page.
```yaml
apiVersion: hobbyfarm.io/v1
kind: PredefinedService
metadata:
  name: code-server-service
  namespace: hobbyfarm-system
spec:
  name: Code-Server Service
  cloud_config: |
    runcmd:
      - export HOME=/root
      - curl -fsSL https://code-server.dev/install.sh > codeServerInstall.sh
      - /bin/sh codeServerInstall.sh && systemctl enable --now code-server@root
      - >
        sleep 5 && sed -i.bak 's/auth: password/auth: none/'
        ~/.config/code-server/config.yaml
      - sudo systemctl restart code-server@root
  has_webinterface: true
  has_tab: true
  port: 8080
  path: /?path=/root
  rewrite_host_header: true
  no_rewrite_root_path: false
  rewrite_origin_header: false
  disallow_iframe: false
```

## Configuration

### `name`
Display name for the Service inside the UI.

**Default:** _null_

### `cloud_config`
Makes use of [cloud-init](https://cloudinit.readthedocs.io/en/latest/) to configure a Service. The cloud-init data is provided as a string and will executed during the initial boot of the virtual machine. Uses the [runcmd](https://cloudinit.readthedocs.io/en/latest/reference/modules.html#runcmd) feature of cloud-init to execute commands.

**Default:** _null_

> **NOTE:** Cloud-Config must be supported by the operator that is used to provision the virtual machine.

### `has_webinterface`
If the service has a web interface which can be accessed by the user, the proxy will connect to the server via ssh and tunnel the web interface located at the configured port to the user.

**Default:** _false_

### `has_tab`
Used in conjunction with `has_webinterface`. If `has_tab` is set to true, the web interface will be get its own tab next to the terminal tabs in the User UI. The tab will be named after the virtual machine and the display name of the Service.

**Default:** _false_

### `port`
The port to which the application is exposed. The proxy will tunnel `localhost:[port]`.

**Default:** _0_

### `path`
Some applications may provide different paths or entrypoint to be called like `/dashboard`. When `path` is defined the first page that is loaded will be the given path.

**Default:** _null_

### `rewrite_host_header`
The HobbyFarm proxy will modify the host header of requests to match the host of the proxy/shell server.

**Default:** _true_

### `no_rewrite_root_path`
The proxy will rewrite the requested path `/p/{virtualMachineId}/80/path/to/application` to `/path/to/application` when calling the server. Set this flag to `true` when you want the proxy so send the whole path.

**Default:** _false_

> **NOTE:** This feature is required by certain applications, such as Jupyter, when providing a `base_url` parameter.

### `rewrite_origin_header`
default: false

The proxy will rewrite the origin header of requests to localhost


### `disallow_iframe`
Disallow the web interface inside iFrame tabs. Users will have to open the exposed web interface inside a new browser tab.

**Default:** _false_

## Additional Example Manifests
Below are additional examples of PredefinedService manifests.
### Docker
The following is an example of a PredefinedService that installs Docker on a VM.
```yaml
apiVersion: hobbyfarm.io/v1
kind: PredefinedService
metadata:
  name: docker-service
  namespace: hobbyfarm-system
spec:
  name: install Docker
  cloud_config: |
    runcmd:
      - curl -fsSL get.docker.com | bash
  has_tab: true
```

### Jupyter
The following is an example of a PredefinedService that installs a Jupyter Notebook on a VM.
```yaml
apiVersion: hobbyfarm.io/v1
kind: PredefinedService
metadata:
  name: jupyter-service
  namespace: hobbyfarm-system
spec:
  name: Jupyter
  port: 8888
  cloud_config: |
    #cloud-config
    runcmd:
      - apt-get update && apt-get install python3-pip -y
      - pip install jupyterlab markupsafe==2.0.1
  has_tab: true
  has_webinterface: true
  path: /
  rewrite_origin_header: true
  disallow_iframe: true
  no_rewrite_root_path: true
```