---
title: Litter-Robot
description: Instructions on how to integrate a Litter-Robot Wi-Fi-enabled, automatic, self-cleaning litter box to Home Assistant.
ha_category:
  - Vacuum
ha_iot_class: Cloud Polling
ha_release: 2021.3
ha_config_flow: true
ha_quality_scale: gold
ha_codeowners:
  - '@natekspencer'
ha_domain: litterrobot
---

The Litter-Robot integration allows you to control and monitor your Wi-Fi-enabled, automatic, self-cleaning litter box for cats.

You will need a Litter-Robot account as well as a Wi-Fi-enabled Litter-Robot unit that has already been associated with your account.

There is currently support for the following device types within Home Assistant:

- Vacuum (this is the representation of your Litter-Robot litter box)

{% include integrations/config_flow.md %}

## Entities

The following entities are created for this component:

| Entity     | Domain   |
| ---------- | -------- |
| Litter Box | `vacuum` |

All of the entities above are grouped together and identified by a single device.

## Attributes

The following additional attributes are available on the `vacuum` component:
| Attribute                     | Type    | Definition                                                                                                                                                         |
| ----------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| clean_cycle_wait_time_minutes | integer | Current wait time, in minutes, between when your cat uses the Litter-Robot and when the unit cycles automatically.                                                 |
| is_sleeping                   | boolean | Whether or not the unit is currently in sleep mode.                                                                                                                |
| power_status                  | string  | Current power status of the unit. `AC` indicates normal power, `DC` indicates battery backup and `NC` indicates that the unit is not connected and/or powered off. |
| unit_status_code              | string  | The [unit status code](https://github.com/natekspencer/pylitterbot/blob/main/pylitterbot/robot.py#L21) associated with the current status of the vacuum.           |
| last_seen                     | string  | UTC datetime the unit last reported its status.                                                                                                                    |

## Commands

In addition to the entities that are created above, some commands are utilized for additional functionality that is available in the Litter-Robot companion app.

### reset_waste_drawer

Resets the waste drawer gauge on the Litter-Robot. This will reset the cycle count returned by the Litter-Robot API to `0`.

```yaml
service: vacuum.send_command
target:
  entity_id: vacuum.litter_robot_litter_box
data:
  command: reset_waste_drawer
```

### set_sleep_mode

Enables (with `sleep_time` param) or disables sleep mode on the Litter-Robot.

| Param      | Type   | Required                                        | Description                                                                                                                                                                                                                                                                                                                                              |
| ---------- | ------ | ----------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| enabled    | bool   | yes                                             | Set to true to enable and false to disable.                                                                                                                                                                                                                                                                                                              |
| sleep_time | string | Required if the param `enabled` is set to true. | Time at which the unit will enter sleep mode and prevent an automatic clean cycle for 8 hours. This param uses the 24-hour format string `%H:%M:%S`, with seconds being optional, and is based on the timezone configured for your Home Assistant installation. As such, `10:30:00` would indicate 10:30 AM, whereas `22:30:00` would indicate 10:30 PM. |

Example of setting the sleep mode to begin at 10:30 PM.

```yaml
service: vacuum.send_command
target:
  entity_id: vacuum.litter_robot_litter_box
data:
  command: set_sleep_mode
  params:
    enabled: true
    sleep_time: "22:30:00"
```
