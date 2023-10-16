+++
title = "Scenario"
description = "A set of steps that are presented to a user during a session."
+++

Scenarios are the main content resource in HobbyFarm. They contain a series of steps which are presented to the end-user during a session. Scenarios also configure what [VirtualMachineTemplates](/docs/architecture/resources/virtualmachinetemplate) are necessary for use of the scenario.

## Example Scenario Manifest
```yaml
apiVersion: hobbyfarm.io/v1
kind: Scenario
metadata:
    name: example-scenario
    namespace: hobbyfarm-system
spec:
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

### `name`
Display name for the scenario as shown in both the end-user UI and admin UI. This field requires base64 encoding.

### `description`
A brief description of the scenario shown under the title in the end-user UI. This field requires base64 encoding.

### `steps`
Contains a list of steps to be displayed to the end-user. The order of the steps in the list is the order in which they will be displayed. Each step is an object containing two fields, `title` and `content`. This field requires base64 encoding.

> **NOTE:** A step within the platform delineates a specific task in a scenario. For instance, an initial step might involve echoing "Hello World" to the CLI, followed by a step that writes "Hello World" to a file using the echo command, and concluding with a step that employs the cat command to display the file's content to stdout.

### `categories`
Lists categories to which the scenario belongs. Categories are used in [Courses](/docs/architecture/resources/course) to dynamically include scenarios based on queries.

### `tags`
Lists tags applied to the Scenario. Tags are used in the admin UI to quickly search for scenarios matching desired content.

### `virtual_machines`
This field contains a `map[string]string` that outlines all the virtual machines needed for a specific scenario, indexed by their names. Each key represents the name of a virtual machine, a label that is consistent in the User UI and within the variable content throughout the steps. The corresponding value for each key identifies the [VirtualMachineTemplate](/docs/architecture/resources/virtualmachinetemplate) associated with that particular virtual machine, serving as its blueprint.

### `keepalive_duration`
Defines the duration of time after which a user's virtual machines will be destroyed by HobbyFarm upon user inactivity. For example, should a user become inactive, the platform will wait for the duration of time defined in this field before destroying the deployed virtual machines.

Acceptable values for this field are of the form `{x}m` or `{x}h` where `{x}` is a positive integer that denotes minutes or hours of time.

```yaml
## Example keepalive_duration value of 90 minutes
keepalive_duration: 90m

## Example keepalive_duration value of 4 hours
keepalive_duration: 4h
```

**Default:** _10m_

### `pause_duration`
Defines the duration of time a user is able to pause an active session.

The User UI contains a button that, if enabled, allows a user to stop the keepalive countdown. This permits the user to take actions that otherwise may result in VM teardown, such as closing their laptop or disconnecting the Internet connection.

If a user pauses the session, the platform will wait until this duration expires before reenabling the keepalive duration.

Acceptable values for this field are of the form `{x}m` or `{x}h` where `{x}` is a positive integer that denotes minutes or hours of time.

> **NOTE:**  Thus the maximum time a user can be paused **_and_** inactive is the sum of `keepalive_duration` and `pause_duration`.

```yaml
## Example pause_duration value of 90 minutes
pause_duration: 90m

## Example pause_duration value of 4 hours
pause_duration: 4h
```

**Default:** _1h_

### `pauseable`
This field determines whether an end-user may pause an active session. If `true`, the end-user may pause the session. If `false`, the end-user may not pause the session.

> **NOTE:** The toggle only applies to sessions which the scenario applies.