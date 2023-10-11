+++
title = "Configure Settings"
weight = 5

+++

## Settings
HobbyFarm has some adjustable settings. They alter the behaviour of the plattform by adding or removing features via feature-gates or by setting numbers like the retention time of certain objects. All settings will be dynamically rendered in the Admin-UI.

Settings always have a name and a scope. The scope defines who can adjust this settings (at least this is the plan) as well as who can retreive the settings (Users will not see all the settings but only public available settings)

Values can be of one of four types:
* String
* Integer
* Float
* Boolean

As well as of three different representations
* Scalar (single value)
* Array
* Map

Settings might also have Validators.
Validators include:
* required
* maximum
* minimum
* maxLength
* minLength
* format
* pattern
* enum
* default
* uniqueItems

See [https://github.com/hobbyfarm/gargantua/blob/master/pkg/property/types.go](Property Types) for a full List and corresponding data types.

## Available Scopes
* public **Accessible without registration**
* ui (Accessible by registered users)
* admin-ui (Accessible by admin only)

Scopes can also be created by operators or any HobbyFarm compatible application.
Therefore the AWS Provisioning Operator could bring the `aws` scope.

The Scope will be saved in label `hobbyfarm.io/setting-scope`

## Available Settings
HobbyFarm brings these settings by default. They are automatically created when missing.

|Name|Display Name|Scope|Default value|
|----|------------|-----|-------------|
|motd-admin-ui|Admin UI MOTD|admin-ui|none|
|motd-ui|User UI MOTD|public|none|
|registration-disabled|Registration disabled|public|false|


## Example Setting Manifest
### Scalar 
```yaml
apiVersion: hobbyfarm.io/v1
dataType: boolean
kind: Setting
metadata:
  labels:
    hobbyfarm.io/setting-scope: public
  name: registration-disabled
  namespace: hobbyfarm
value: "true"
valueType: scalar
```

### Array with validators
```yaml
apiVersion: hobbyfarm.io/v1
dataType: string
displayName: String Array
kind: Setting
metadata:
  labels:
    hobbyfarm.io/setting-group: my-group
    hobbyfarm.io/setting-scope: public
  name: string-array
value: '["string","array"]'
valueType: array
minLength: 2
maxLength: 4
```

### Enum
```yaml
apiVersion: hobbyfarm.io/v1
dataType: string
kind: Setting
metadata:
  labels:
    hobbyfarm.io/setting-scope: public
  name: enum-setting
  namespace: hobbyfarm
value: "abc"
enum: ["xyz", "abc", "def"]
valueType: scalar
```