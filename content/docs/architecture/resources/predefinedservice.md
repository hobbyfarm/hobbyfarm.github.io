+++
title = "PredefinedService"
+++

A `PredefinedService` is a way to provide preconfigured Services that are usible when creating or updating `VirtualMachineTemplates` inside the Admin-UI. 

Example manifest:
Look at the bottom of page for more Example Services.
```yaml
apiVersion: hobbyfarm.io/v1
kind: PredefinedService
metadata:
  name: another-service
  namespace: hobbyfarm-system
spec:
  name: install Code-Server IDE
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

### `cloud_config`
default: "" (empty string)


Cloud-Config data. See [documentation]https://cloudinit.readthedocs.io/en/latest/

> This has to be supported by the operator or the terraform module. 

### `has_webinterface`
default: false

If the service provides a Webinterface that can be proxied to the user. The proxy will connect to the server via ssh and tunnel the webinterface located at the configured port to the user.

### `has_tab`
default: false

All webinterfaces are grouped inside a submenu next to the terminal tabs.
If `has_tab` is set to true the webinterface will be get its own tab next to the terminal tabs. The tab will be named after the virtual machine and the display name of the service.

### `port`
default: 0

The port to which the application is exposed. The proxy will tunnel `localhost:[port]`.

### `path`
default: "" (empty string)

Some applications may provide different paths or entrypoint to be called like `/dashboard`. When `path` is defined the first page that is loaded will be the given path.

### `rewrite_host_header`
default: true

The proxy will rewrite the host header of requests to the proxy/shell server host.

### `no_rewrite_root_path`
default: false

The proxy will rewrite the requested path `/p/[vm-id]/80/path/to/application` to `/path/to/application` when calling the server. Set this flag to `true` when you want the proxy so send the whole path. This is needed by some applications like Jupyter when providing a `base_url` parameter.

### `rewrite_origin_header`
default: false

The proxy will rewrite the origin header of requests to localhost


### `disallow_iframe`
default: false

Disallow the webinterface inside iFrame tabs, the user will have to open the webinterface inside a new tab.

## Example PredefinedServices:
### Docker
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
### Code-Server
```yaml
apiVersion: hobbyfarm.io/v1
kind: PredefinedService
metadata:
  name: code-server-service
  namespace: hobbyfarm-system
spec:
  name: Code-Server IDE
  port: 8080
  cloud_config: |
    runcmd:
      - export HOME=/root
      - curl -fsSL https://code-server.dev/install.sh > codeServerInstall.sh
      - /bin/sh codeServerInstall.sh && systemctl enable --now code-server@root
      - >
        sleep 5 && sed -i.bak 's/auth: password/auth: none/'
        ~/.config/code-server/config.yaml
      - sudo systemctl restart code-server@root
  has_tab: true
  has_webinterface: true
  path: /?path=/root
  rewrite_host_header: true
  no_rewrite_root_path: false
  rewrite_origin_header: false
  disallow_iframe: false
``` 
### Jupyter

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
```