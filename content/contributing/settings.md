+++
title = "Settings"
description = "Enable/disable HobbyFarm features by adjusting settings."
+++

HobbyFarm makes use of adjustable settings to alter the behavior of the platform by adding or removing features via feature-gates or by setting variables, such as the retention time of specific objects.

All settings are dynamically rendered in the Admin-UI under the [Settings](/docs/configuration/settings) page under Configuration.

## Settings
Settings always have a `name` and a `scope`. The name provides a unique identifier for the setting while the scope defines who can adjust settings as well as who can retrieve the settings.

### Default Settings
HobbyFarm generates the following default settings. If these settings are removed, they will be automatically recreated.

| Name | Display Name | Scope | Default Value |
| --- | --- | --- | --- |
| motd-admin-ui | Admin UI MOTD | admin-ui | none |
| motd-ui | User UI MOTD | public | none |
| registration-disabled | Registration disabled | public | false |

### Settings dataTypes
Settings `dataType` values can be of one of four types:
| dataType | Example | Description |
| --- | --- | --- |
| string | "Hello World" | A sequence of characters. |
| integer | 123 | A whole number. |
| float | 123.456 | A number with a decimal point. |
| boolean | true | A value which is either `true` or `false`. |

### Settings valueTypes
Settings `valueType` values can be one of three types:
| valueType | Example | Description |
| --- | --- | --- |
| scalar | "Hello World" | A single value. |
| array | ["Hello", "World"] | A list of values. |
| map | {"key": "value"} | A key/value pair. |

### Settings Validators
Settings may also include Validators.

| Validator | Example | Description |
| --- | --- | --- |
| required | true | The value must be set. |
| maximum | 100 | The value must be less than or equal to the maximum value. |
| minimum | 0 | The value must be greater than or equal to the minimum value. |
| maxLength | 10 | The value must be less than or equal to the maximum length. |
| minLength | 0 | The value must be greater than or equal to the minimum length. |
| format | "ipv4" | The value must match the format. |
| pattern | "^[a-z]+$" | The value must match the pattern. |
| enum | ["a", "b", "c"] | The value must be one of the enum values. |
| default | "Hello World" | The value will be set to the default value if not set. |
| uniqueItems | true | The value must be unique. |

> Please see the [Gargantua Property Types](https://github.com/hobbyfarm/gargantua/blob/master/pkg/property/types.go) for a full list of and corresponding data types.

### Example Setting Manifests
```yaml
## Boolean dataType using a scalar.
## Assigned to the public scope.
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

---

## String dataType using an array with validators.
## Assigned to the admin-ui scope.
apiVersion: hobbyfarm.io/v1
dataType: string
displayName: String Array
kind: Setting
metadata:
  labels:
    hobbyfarm.io/setting-group: my-group
    hobbyfarm.io/setting-scope: admin-ui
  name: string-array
value: '["string","array"]'
valueType: array
minLength: 2
maxLength: 4

---

## String dataType using an enum.
## Assigned to the user-ui scope.
apiVersion: hobbyfarm.io/v1
dataType: string
kind: Setting
metadata:
  labels:
    hobbyfarm.io/setting-scope: user-ui
  name: enum-setting
  namespace: hobbyfarm
value: "abc"
enum: ["xyz", "abc", "def"]
valueType: scalar
```

## Scopes
The following scopes are available in HobbyFarm by default:

| Scope | Description |
| --- | --- |
| public | Settings which are accessible without registration. |
| user-ui | Settings which are accessible by registered users. |
| admin-ui | Settings which are accessible by administrators. |

### Scope Notes

* Scopes can also be created by operators or any HobbyFarm compatible application. For example, an AWS Provisioning Operator could generate an `aws` scope.
* A Scope is applied to a Setting via a label using the key `hobbyfarm.io/setting-scope` with the value of the scope name. For example:
  * `hobbyfarm.io/setting-scope: public`
  * `hobbyfarm.io/setting-scope: user-ui`
  * `hobbyfarm.io/setting-scope: admin-ui`

### Example Scope Manifests
```yaml
## Public Scope
apiVersion: hobbyfarm.io/v1
displayName: Public
kind: Scope
metadata:
  name: public
  namespace: hobbyfarm-system

---

## User UI Scope
apiVersion: hobbyfarm.io/v1
displayName: User UI
kind: Scope
metadata:
  name: user-ui
  namespace: hobbyfarm-system

---

## Admin UI Scope
apiVersion: hobbyfarm.io/v1
displayName: Admin UI
kind: Scope
metadata:
  name: admin-ui
  namespace: hobbyfarm-system
```