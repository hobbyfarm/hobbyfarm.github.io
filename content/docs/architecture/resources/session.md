+++
title = "Session"
description = "A user currently executing a scenario or course."
+++

The `Session` resource in HobbyFarm tracks one or more users execution of a scenario or course, storing information such as the start time of the user, which scenario or course is being executed, etc.

> **NOTE:** :warning: Sessions are created by HobbyFarm and **_SHOULD NOT_** be modified and/or created manually. This documentation is provided for informational purposes only.

## Kubernetes Commands
The following commands are useful for managing Session resources in Kubernetes.

```bash
## Get a list of all Sessions
kubectl get sessions -n hobbyfarm-system

## Delete a Session
kubectl delete session {sessionName} -n hobbyfarm-system
```

## Example Session Manifest
The following is an example of a Session resource in Kubernetes.

```yaml
apiVersion: hobbyfarm.io/v1
kind: Session
metadata:
    name: sample-session
    namespace: hobbyfarm-system
spec:
    scenario: s-lkjsdflkjs
    course: c-lkjsdflkjs
    keep_course_vm: "true"
    user: u-lkjsdflkjs
    vm_claim: vmc-lkjsdflkjsf
    access_code: ac-ljw4s4dsdf
status:
    paused: "false"
    pause_time: ""
    active: "true"
    finished: "false"
    start_time: "2006-01-02 15:04:05.999999999 -0700 MST"
    end_time: "2006-01-02 17:04:05.999999999 -0700 MST"
```

## Specification Configuration

### `scenario`
The identification of the scenario currently in use by the user.

### `course`
If the user is executing a scenario in the context of a course, this field tracks the course identification.

### `keep_course_vm`
Determines if HobbyFarm should/shouldn't keep the virtual machines referenced by the session for the duration of the Course. If `true`, the virtual machines are kept. If `false`, the virtual machines are deleted when the user finishes the session.

### `user`
The identification of the user that owns the session.

### `vm_claim`
The identification of the VirtualMachineClaim owned by the session.

### `access_code`
The identification of the AccessCode that granted access to the scenario/course.

## Status Configuration

### `paused`
Shows if the session has been paused by the user. If `true`, the session is paused. If `false`, the session is not paused.

### `pause_time`
A time stamp marking when the user paused the session.

### `active`
Shows if the session is currently active. If `true`, the session is active. If `false`, the session is inactive.

### `finished`
Shows if the session has concluded. If `true`, the session has concluded. If `false`, the session is still active.

### `start_time`
The timestamp when the session was created.

### `end_time`
The timestamp after which point the session expires and is concluded.

> **NOTE:** The timestamp is continuously updated as the end-user UI sends "keep-alive" messages to the HobbyFarm API endpoint. Every keep-alive message advances the timestamp. This prevents an active user from getting disconnected, but more importantly allows HobbyFarm to reclaim resources from users who have closed their browsers or otherwise gone inactive.