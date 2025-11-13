<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# 🚨 Alert Data Sensor

This blueprint helps you create a template sensor that stores the data of active alert automations created using the [Telegram alert blueprint](/telegram/telegram_alert.md).

By using this template sensor, alerts can be continued after a restart of HA or a reload of the alert automation. If the sensor is not used, the alert will start over from scratch.  
The alert automation also has functionality to remove old messages from the Telegram chat, which will only work for messages sent before a HA restart or automation reload if this sensor is used.

## ❓ How to Use

You can import the blueprint using the button below. Note that this blueprint will not show up under Settings > Automations & Scenes > Blueprints. Also, it needs to be set up using YAML, as there is currently no GUI support for template blueprints.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fother%2Falert_data_sensor.yaml)

After that, you'll need to edit your YAML configuration. In case you are using HAOS, you can use a file editor add-on like [Visual Studio Code Add-on](https://my.home-assistant.io/redirect/supervisor_addon/?addon=a0d7b954_vscode) or the [File Editor Add-on](https://my.home-assistant.io/redirect/supervisor_addon/?addon=core_configurator).

If you have no previous template entities defined using YAML, you can copy the following YAML to the bottom of your `configuration.yaml` file.  
If you already have YAML-defined template entities (using the modern YAML syntax), make sure to add it under the existing `template:` key. If you include a file there, like `template.yaml`, you can put it in that file. Ensure you remove the first line (`template:`) and that the indentation matches the already existing items in that file.

You only need to do this once; it will store the data of all the Telegram alert blueprint automations.

```yaml
# Basic alert data sensor configuration
template:
  - use_blueprint:
      path: TheFes/alert_data_sensor.yaml
    name: thefes.casa alert data
    unique_id: b6fcfc77-8ffc-434c-87c0-1824b1e990dc
```
After a reload of Template entities in [Developer tools > YAML](https://my.home-assistant.io/redirect/server_controls/) or a restart of Home Assistant (in case `Template entities` isn't shown in the list in Developer Tools > YAML), the sensor is created.
By default, the sensor will have the entity_id `sensor.thefes_alert_data`, which will listen to events with event_type `update_thefes_alert_sensor`. Optionally, you can change those, but make sure to update the settings in the alert automations as well. Also, make sure to reload template entities again if you make changes.

```
# Configuration with optional input
template:
  - use_blueprint:
      path: TheFes/alert_data_sensor.yaml
      input:
        trigger_event: update_thefes_alert_sensor
        entity_id: sensor.thefes_alert_data
    name: thefes.casa alert data
    unique_id: b6fcfc77-8ffc-434c-87c0-1824b1e990dc
```

## ⚠️ NOTE:
* If you change the entity_id in the GUI settings, make sure to change it as well in the settings above; otherwise, the sensor will no longer work.
* To limit the number of options in the alert automations, I've added the device class `current` and unit of measurement `A` (for Alert in this case 😉). So if you have some template sensors that e.g. calculate an average of all sensors with device class `current`, this one might be included. If that's the case, exclude it in your templates.