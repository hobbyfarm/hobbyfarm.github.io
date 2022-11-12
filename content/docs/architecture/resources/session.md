+++
title = "Session"
+++

In HobbyFarm, a session is defined as a user currently executing a scenario or course. The Session resource in HobbyFarm tracks that occurrence, storing information such as the start time of the user, which scenario or course is being executed, etc. 

> :warning: Sessions are always created by HobbyFarm's code. Except in very rare, surgical operations, you should not create a Session resource via the Kubernetes API. You may view the Session in this manner but creating one outside of HobbyFarm's code is not a supported operation. 

Here's an example Session:

```yaml
apiVersion: hobbyfarm.io/v1
kind: Session
metadata:
    name: sample-session
    namespace: hobbyfarm-system
spec:
    id: sample-session
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

## Configuration

### `id`


Identifier for the session. Should be identical to the Kubernetes `metadata.name` field. Present for historical reasons, will be phased out in a future release. 

> This field is currently *required* in HobbyFarm. Architecturally it is considered deprecated and may be removed in a future release. For now, users must continue to set this field. 

### `scenario`

Id of the scenario that the user is currently executing. 

### `course`

If the user is executing a scenario in the context of a course, this field tracks the course id.

### `keep_course_vm`

Whether or not HobbyFarm should keep the VMs referenced by this session for the duration of the Course. 

### `user`

Id of the user that owns this session.

### `vm_claim`

Id of the VirtualMachineClaim that this session owns. 

### `access_code`

Id of the AccessCode that granted access to this scenario/course.

### `status.paused`

Whether or not the user has paused the session.

### `status.pause_time`

Time stamp marking when the user paused their session.

### `status.active`

Whether or not this session is active. 

### `status.finished`

Whether or not this session has concluded.

### `status.start_time`

The timestamp when this session was created.

### `status.end_time`

The timestamp after which point the session expires and is concluded. This timestamp is continuously updated as the end-user UI sends "keepalive" messages to the HobbyFarm API endpoint. Every keepalive message advances the timestamp. This prevents an active user from getting disconnected, but more importantly allows HobbyFarm to reclaim resources from users who have closed their browsers or otherwise gone inactive.