+++
title = "ScheduledEvent"
description = "A collection of Scenarios and Courses that are scheduled to run at a specific time."
+++

In HobbyFarm, a `ScheduledEvent` is a collection of [Scenarios](/docs/architecture/resources/scenario) and [Courses](/docs/architecture/resources/course) that are scheduled to run at a specific time. ScheduledEvents make use of an access code to restrict access to the event.

Users must enter the access code to view and initiate the event. The access code is created during the creation of the ScheduledEvent and can be changed at any time. A ScheduledEvent can be configured to provision virtual machines on-demand or at a specific time.

A ScheduledEvent can be created by an administrator in the Admin-UI under the `Scheduled Events` tab.

> **NOTE:** ScheduledEvents should be created using the Admin-UI, which creates the ScheduledEvent resource. It is not recommended to create ScheduledEvent resources manually.

## Kubernetes Commands
The following commands are useful for managing ScheduledEvent resources in Kubernetes.

```bash
## Get a list of all ScheduledEvents
kubectl get scheduledevents -n hobbyfarm-system

## Create a ScheduledEvent from a YAML manifest
kubectl apply -f {scheduledEventManifest} -n hobbyfarm-system

## Edit a ScheduledEvent
kubectl edit scheduledevent {scheduledEventName} -n hobbyfarm-system

## Backup a ScheduledEvent to a YAML manifest
kubectl get scheduledevent {scheduledEventName} -n hobbyfarm-system -o yaml > {scheduledEventManifest}

## Delete a ScheduledEvent
kubectl delete scheduledevent {scheduledEventName} -n hobbyfarm-system
```

## Example ScheduledEvent Manifest
The following shows an example of a ScheduledEvent manifest in Kubernetes.

```yaml
apiVersion: hobbyfarm.io/v1
kind: ScheduledEvent
metadata:
  name: se-se-22kaloamft
  namespace: hobbyfarm-system
spec:
  access_code: "12345"
  courses:
  - c-6ovfvaanef
  creator: admin
  description: A description of the demo scheduled event.
  end_time: Mon Oct 23 15:10:04 UTC 2023
  event_name: Demo Event
  on_demand: true
  printable: true
  required_vms:
    env-vu7rst7izz:
      vmt-neuc2clbtv: 10
  restricted_bind: true
  restricted_bind_value: se-se-22kaloamft
  scenarios:
  - s-kphtuenlbk
  start_time: Mon Oct 16 15:10:03 UTC 2023
```

## Configuration

### `access_code`
The access code required for users to view and initiate the ScheduledEvent.

> **NOTE:** The access code must be a string of 5 characters or more.

### `courses`
A list of Courses that are included in the ScheduledEvent. The courses are listed by the course ID.

> **NOTE:** A course must be created before it can be added to a ScheduledEvent.

### `creator`
The name of the user who created the ScheduledEvent.

### `description`
A description of the ScheduledEvent.

### `end_time`
The time when the ScheduledEvent ends. Used in conjunction with `start_time` to define the time range of the ScheduledEvent.

> **NOTE**: All virtual machines are destroyed when the ScheduledEvent ends, if if the ScheduledEvent is deleted before the end time.

### `event_name`
The name of the ScheduledEvent. This name is displayed to users when they view the ScheduledEvent.

### `on_demand`
Allocates virtual machine resources when requested by a user instead of pre-provisioning the resources. When set to `true`, virtual machine resources are allocated when a user initiates a session on the ScheduledEvent. When set to `false`, virtual machine resources are allocated based on the start time of the ScheduledEvent.

**Default:** _true_

### `printable`
Enables the ability for users to print the content of a scenario and/or save it as a PDF file.

**Default:** _false_

### `required_vms`
A list of required virtual machines which can be used by the ScheduledEvent. The list is defined as a map of environments and associated virtual machine templates, along with the number of virtual machines that are required.

> **NOTE:** Multiple environments can be defined in the same ScheduledEvent. The environments are listed by the environment ID. Multiple virtual machine templates can be defined in an environment. The virtual machine templates are listed by the virtual machine template ID.

### `restricted_bind`
Prevents users from reserving virtual machine resources that are not part of the ScheduledEvent. When set to `true`, users can only reserve virtual machine resources that are part of the ScheduledEvent. When set to `false`, users can reserve any virtual machine resource.

**Default:** _true_

### `restricted_bind_value`
Binds virtual machine resources to the ScheduledEvent. This is set to the name of the ScheduledEvent.

### `scenarios`
A list of Scenarios that are included in the ScheduledEvent. Scenarios are listed by the scenario ID.

> **NOTE:** A Scenario must be created before it can be added to a ScheduledEvent.

### `start_time`
The time when the ScheduledEvent starts. Used in conjunction with `end_time` to define the time range of the ScheduledEvent.
