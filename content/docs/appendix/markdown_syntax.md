+++
title = "Special Markdown Syntax"
description = "Reference document for the special markdown syntax available in the scenario content."
+++

This page contains a reference of the special markdown syntax available in the scenario contents. 

## Code
### Syntax Highlighting
You can enable syntax highlighting by providing a language to the codeblock. Providing the optional filename will display it together with the content

````
```<language>:<optional filename>
code
```
````

For example

````
```md
# Content
```

```yaml:myfile.yaml
yaml: highlight this #comment
```
````

### Click-To-File
Beginning with HobbyFarm 2.0.6 you can also enable users to create files directly with one click using the folowing syntax:

````
```file:<language>:<filepath>:<target>
content
```
````

For example:
````
```file:yaml:/var/myfile.yaml:node1
content: yaml content
```
````

### Click-To-Run
 Click-to-run is enabled by passing a special language argument to a markdown code block. Use the syntax `ctr:<vmname>[:<limit>]` as the language argument where vmname is the name of one of the VMs in the scenario. and limit is the maximum number of times the CTR can be pressed to be executed. Limit can be omitted which defaults to unlimited executions.

Examples:
````
```ctr:cluster01
cat example.txt
```

```ctr:cluster01
echo This CTR has no limit
```

```ctr:cluster01:2
echo This CTR limits to 2 clicks
```
````

## Virtual Machine Information
 The listed variables are available. To use, specify within a ${} block using the following syntax:

`${vminfo:[vmname]:[variable]}` where vminfo is a static string, vmname is the name of one of the VMs in the scenario, and variable is an item from the list on the right.

Example: `${vminfo:cluster01:public_ip}`

### Available Options
all fields that are provided by virtual machines
* id
* vm_template_id
* keypair_name
* vm_claim_id
* user
* status
* allocated
* tainted
* public_ip
* private_ip
* environment_id
* hostname
* tfstate
* ws_endpoint
* vm_set_id
* protocol


## Session Information
Display the session id by using `${session}`.

## Summary / Hidden / Solution Text Element
 To create a hidden text element, pass the special language argument hidden to a markdown code block.

Example:
````
```hidden:Hidden Text Summary
This is the hidden Text that opens and closes after a click on the summary
```
````

## Glossary
 To create a glossary element, pass the special language argument `glossary:<name>` to a markdown code block. The text inside the code block will be displayed when a user hovers over the provided name

Example:
````
```glossary:Kubernetes
Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.
```
````

## Nested Elements
Nested elements can be applied inside blocks or hidden elements. To define a nested block, replace backticks by tildes.

Example:
````
```
Some text
~~~python
# This program prints Hello, world!

print('Hello, world!')
~~~
Some more text
```
````

## Note
as of HobbyFarm 2.0.6 Notes can be used to highlight parts of the text with a special box.

````
```note:<type>:<optional title>
Content
```
````

Examples:
````
```note:info:More info
Note that is suitable for additional `information`
```

```note:task
Check the nodes.
~~~ctr:node1
kubectl get nodes
~~~
```

```note:other
Unknown note type. Can be used for general boxes
```
````

### Types
* info (Blue)
* warn (Orange)
* caution (Red)
* task (Green)
all other values than the types state above will result in a grey highlighted box
