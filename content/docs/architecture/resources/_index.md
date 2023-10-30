+++
title = "K8s Resources"
description = "Custom Kubernetes resources used by HobbyFarm."
+++

In Kubernetes, a resource is an endpoint in the Kubernetes API that stores a collection of API objects of a certain kind. For example, the built-in [Pod](https://kubernetes.io/docs/concepts/workloads/pods/) resource contains a collection of `Pod` objects.

A [custom resource](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) is an extension of the Kubernetes API that is not necessarily available in a default Kubernetes installation. A custom resource is a Kubernetes API object that is defined and implemented by the user.

HobbyFarm uses custom resources to define the various objects used by HobbyFarm. All configurations and changes made in the HobbyFarm UI are stored in Kubernetes as a resource, and can be managed via `kubectl` or 
 the documentation provides information on the custom resources used by HobbyFarm.

```bash
## Get a list of HobbyFarm resources
kubectl api-resources --api-group=hobbyfarm.io
```

