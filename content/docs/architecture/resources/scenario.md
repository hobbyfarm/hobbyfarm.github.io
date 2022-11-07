+++
title = "Scenario"
+++

Scenarios are the main content resource in HobbyFarm. They contain a series of steps which are presented to the end-user during a session. Scenarios also configure what VM templates are necessary for use of the scenario.

Here's an example scenario:
```yaml
apiVersion: hobbyfarm.io/v1
kind: Scenario
metadata:
    name: example-scenario
    namespace: hobbyfarm-system
spec:
    id: example-scenario
    name: RXhhbXBsZSBTY2VuYXJpbwo= # "Example Scenario"
    description: VGhpcyBpcyBhbiBleGFtcGxlIHNjZW5hcmlvCg== # "This is an example scenario"
    steps:
    - title: U3RlcCAxCg== # "Step 1"
      content: VGhpcyBpcyB0aGUgY29udGVudCBmb3Igc3RlcCAxCg== # "This is the content for step 1"
    - title: U3RlcCAyCg== # "Step 2"
      content: VGhpcyBpcyB0aGUgY29udGVudCBmb3Igc3RlcCAyCg== # "This is the content for step 2"
    categories:
    - learning
    - cloud-native
    tags:
    - cool-scenario
    - example
    - great-content
    virtual_machines:
        machine01: ubuntu-2004
        cluster01: sles-15-sp4
    keepalive_duration: 90m 
    pause_duration: 4h
    pauseable: true
```

## Configuration

### `id` 

Identifier for the scenario. Should be identical to the Kubernetes `metadata.name` field. Present for historical reasons, will be phased out in a future release. 

> This field is currently *required* in HobbyFarm. Architecturally it is considered deprecated and may be removed in a future release. For now, users must continue to set this field. 

### `name`

Display name for the scenario as shown in both the end-user UI and admin UI. This field should be base64 encoded.

### `description`

A brief description of the scenario shown under the title in the end-user UI. This field should be base64 encoded. 

### `steps`

This field contains a list of steps which (in order) are the steps displayed to the end-user. Each step is an object containing two fields, `title` and `content`. The value of each of these fields should be base64-encoded. 

### `categories`

This field lists categories to which the scenario belongs. Categories are used in the UI to group scenarios with common elements together.

### `tags`

This field lists tags applied to the scenario. Tags are used in the admin UI to quickly search for scenarios matching desired content. 

### `virtual_machines`

This field is a `map[string]string` listing each virtual machine that is required for the scenario. The key is the name of the VM (as used in both the end-user UI *and* the variable content in the steps), and the value is the VM template which "backs" that VM. 

### `keepalive_duration`

This field defines the duration of time after which a user's VMs can be destroyed by HobbyFarm upon user inactivity. In other words, if a user becomes inactive (their instance of the end-user UI stops sending pings to the server) this is the duration of time HobbyFarm will wait before reclaiming their resources. 

Acceptable values for this field are of the form `XXm` or `XXh` where `XX` is a positive integer that denotes minutes or hours of time. 

### `pause_duration`

This field defines the duration of time a user is able to pause their session. The end-user UI contains a button that allows (if enabled) a user to stop the keepalive countdown. This permits the user to take actions that otherwise may result in VM teardown, such as closing their laptop or disconnecting their Internet connection. If a user pauses their session, HobbyFarm will wait until this duration expires before once again reenabling the keepalive duration. Thus the maximum time a user can be paused _and_ inactive is the sum of `keepalive_duration` and `pause_duration`.

Acceptable values for this field are of the form `XXm` or `XXh` where `XX` is a positive integer that denotes minutes or hours of time. 

### `pauseable`

This field determines whether an end-user may pause their session. `true` enables the pausing of sessions, `false` disables it.

This toggle only applies to sessions that are using this scenario. 