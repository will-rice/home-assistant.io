---
title: Philips TV
description: Instructions on how to add Philips TVs to Home Assistant.
ha_category:
  - Media Player
ha_iot_class: Local Polling
ha_release: 0.34
ha_codeowners:
  - '@elupus'
ha_domain: philips_js
ha_config_flow: true
---

The `philips_js` platform allows you to control Philips TVs which expose the [jointSPACE](http://jointspace.sourceforge.net/) JSON-API.

Instructions on how to activate the API and if your model is supported can be found [here](http://jointspace.sourceforge.net/download.html). Note that not all listed, jointSPACE-enabled devices will have JSON-interface running on port 1925. This is true at least for some models before year 2011.

{% include integrations/config_flow.md %}

### Features

| Feature            | 1                | 5   | 6 (Android)        | 6 (Saphi)        |
| ------------------ | ---------------- | --- | ------------------ | ---------------- |
| Power On           | WOL / IR Blaster | ?   | Yes (if always on) | WOL / IR Blaster |
| Volume Detect      | Yes              | ?   | Yes (not over CEC) | ?                |
| Volume Up/Down     | Yes              | ?   | Yes                | No               |
| Volume Set         | Yes              | ?   | Yes                | ?                |
| Source Select      | Yes              | ?   | Yes                | No               |
| Source Detect      | Yes              | ?   | No                 | No               |
| Channel Select     | Yes              | ?   | Yes                | ?                |
| Channel Detect     | Yes              | ?   | Yes                | ?                |
| Channel Favorites  | No               | ?   | Yes                | ?                |
| Application Select | No               | ?   | Yes                | No               |
| Application Detect | No               | ?   | Yes                | No               |
| Browse URL         | No               | No  | No                 | No               |
| Send Key           | No               | No  | No                 | No               |
| Ambilight Control  | No               | No  | No                 | No               |

### Turn on device

The Philips TV does not always support turning on via the API. You can either turn it on via IR blaster or on som models WOL. To trigger this command from the entities, the integration exposes a `device trigger` that can be setup to execute when the `media_player` is asked to turn on.
