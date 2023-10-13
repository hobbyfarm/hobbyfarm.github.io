+++
title = "Course"
description = "A collection of HobbyFarm Scenarios meant to be consumed as a single unit or as a series of individual sessions."
+++

A `Course` is useful in situations where a trainer wants to group related learnings together or provide a day's worth of curriculum to students who can retain their VMs from scenario to scenario.

The Course resource is used to populate content in the HobbyFarm Admin-UI Content Management > Courses page. Modifying a Course resource in Kubernetes will modify the information in the Admin-UI.

## Course Manifest Example
The following shows an example of a Course manifest in Kubernetes.

```yaml
apiVersion: hobbyfarm.io/v1
kind: Course
metadata:
    name: example-course
    namespace: hobbyfarm-system
spec:
    name: RXhhbXBsZSBDb3Vyc2UK # "Example Course"
    description: VGhpcyBpcyBhbiBleGFtcGxlIGNvdXJzZQo= # "This is an example course"
    scenarios:
    - scenario-one
    - scenario-two
    categories:
    - cool-courses
    - day-one-content
    virtual_machines:
        machine01: ubuntu-2004
        cluster01: sles-15-sp4
    keepalive_duration: 90m
    pause_duration: 4h
    pauseable: true
    keep_vm: true
```

## Configuration

### `name`

Display name for the course as shown in both the end-user UI and admin UI. This field should be base64 encoded.

### `description`

A brief description of the course shown under the title in the end-user UI. This field should be base64 encoded.

### `scenarios`

This field defines the scenarios that comprise the course. Scenarios are displayed in the order they are defined in this list. Each entry in this list is the `name` (`id`) of the scenario.

### `categories`

This field allows a course to select scenarios _dynamically_ based on the categories applied to the scenario. A basic query format is used to match categories of scenarios. Any scenarios that match are included with the course _in addition to any scenarios defined in the [scenarios](#scenarios) field.

From the admin UI, here are some example query strings:

|Query|Effect|
|-----|------|
|`kubernetes`|All scenarios with category `kubernetes` are included.|
|`!kubernetes`|All scenarios that are not in the `kubernetes` category are included.|
|`kubernetes&basic`|Scenarios that have _both_ the `kubernetes` _and_ `basic` categories are included.|
|`kubernetes&k3s&basic`|Queries can work with more than two categories.|
|`kubernetes&!basic`|All Scenarios that are in the `kubernetes` category, but not in the `basic` category.|
|`example&!example`|This query will never match any scenarios.|

### `virtual_machines`

This field is a `map` listing each virtual machine that is required for the course. The key is the name of the VM (as used in both the end-user UI *and* the variable content in the steps), and the value is the VM template which "backs" that VM.

This field must define all VMs which are used for the scenarios that this course contains. A scenario may fail to execute if the course does not define a matching VM in this map for that scenario.

### `keepalive_duration`

This field defines the duration of time after which a user's VMs can be destroyed by HobbyFarm upon user inactivity. In other words, if a user becomes inactive (their instance of the end-user UI stops sending pings to the server) this is the duration of time HobbyFarm will wait before reclaiming their resources.

Acceptable values for this field are of the form `XXm` or `XXh`, where `XX` is a positive integer that denotes minutes or hours of time.

### `pause_duration`

This field defines the duration of time a user is able to pause their session. The end-user UI contains a button that allows (if enabled) a user to stop the keepalive countdown. This permits the user to take actions that otherwise may result in VM teardown, such as closing their laptop or disconnecting their Internet connection. If a user pauses their session, HobbyFarm will wait until this duration expires before once again reenabling the keepalive duration. Thus the maximum time a user can be paused _and_ inactive is the sum of `keepalive_duration` and `pause_duration`.

Acceptable values for this field are of the form `XXm` or `XXh` where `XX` is a positive integer that denotes minutes or hours of time.

### `pauseable`

This field determines whether an end-user may pause their session. `true` enables the pausing of sessions, `false` disables it.

This toggle only applies to sessions that are using this course.

### `keep_vm`

This field defines whether HobbyFarm should spawn VMs independently for each scenario in the course, or a "persistent" set of VMs that live for the duration of the course and are reused in each scenario. 

Setting this field to `true` causes HobbyFarm to create persistent VMs that live for the life of the course.

Setting this field to `false` causes HobbyFarm to spawn (or claim, depending on provisioning method) unique VMs for each scenario. 