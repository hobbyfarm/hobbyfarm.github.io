+++
title = "Settings"
description = "Configure general HobbyFarm settings."
weight = 1
+++

The Settings page allows administrators to configure general settings for the HobbyFarm platform. To access the Settings page, log into the HobbyFarm Admin-UI and click the `Configuration` option in the top navigation menu. In the left navigation menu, click the `Settings` option.

Settings are divided into the following `scopes`:

## Public
The `Public` scope is used to configure settings which impact the HobbyFarm platform as a whole.

![Settings - Scope - Public](/images/hobbyfarm-admin-settings-public.png)

### User UI MOTD
A message to be displayed to users when logging into the HobbyFarm User UI.

**Default:** Null

#### Registration Disabled
Disables the ability for users to register for a HobbyFarm account. This is useful when the HobbyFarm platform is being used in a closed environment with users who are already known and created by an administrator in the HobbyFarm platform.

**Default:** False

## Admin UI
The `Admin UI` scope is used to configure settings which impact the HobbyFarm Admin UI.

![Settings - Scope - Admin UI](/images/hobbyfarm-admin-settings-adminui.png)

### Admin UI MOTD
A message to be displayed to administrators when logging into the HobbyFarm Admin UI.

**Default:** Null

## User UI

Currently, no settings are available for the `User UI` scope.

## Gargantua

The `Gargantua` scope is used to configure settings which impact the Gargantua backend service.

![Settings - Scope - Admin UI](/images/hobbyfarm-admin-settings-gargantua.png)

### ScheduledEvent Retention Time (h)

The amount of time, in hours, that a scheduled event will be retained in the Gargantua database. After this time, the scheduled event will be deleted from the database.

**Default:** 24