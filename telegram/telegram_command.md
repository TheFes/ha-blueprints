<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# 💬 Telegram command blueprint

This blueprint creates an automation to use custom commands in Telegram, including a `/help` command to show all available commands, and commands to restart and update Home Assistant components.

By default the following commands are added

`/help`: This will display all available commands with a description. The descriptions can be provided and/or adjusted in the blueprint settings

<img alt="telegram alert example image" src="https://github.com/TheFes/ha-blueprints/blob/main/images/telegram_command_help_example.png">

`/restart`: This command will show Home Assistant components which can be restarted. By default an option to restart Home Assistant Core is added. For each component an argument is provided (for Home Assistant Core this is 
`core`) which can be added as argument to prevent the first message asking which component to restart. For example `/restart core` will immediately indicate that you want to restart the Home Assistant Core component. Optionally you can let the bot ask for confirmation before the restart command is issued. This is enabled by default. In case no components to restart are provided, so when the default Home Assistant Core option is removed, and no others are added, the command is disabled.

<img alt="telegram alert example image" src="https://github.com/TheFes/ha-blueprints/blob/main/images/telegram_command_restart_example.png">

`/update`: This command will list all pending updates in Home Assistant, and for each of them show some details and buttons to either install or skip the update. You can optionally provide entities or integrations which should not be used for this command. All related text can optionally be amended or translated.

<img alt="telegram alert example image" src="https://github.com/TheFes/ha-blueprints/blob/main/images/telegram_command_update_example.png">

There is also an option to provide custom commands. By default the `/ping` command is added, which will simply return `Pong 🏓` and can be used to test if the automation is working.
All custom commands can return a message, and or perform actions.

***

## ❓How to use

Use the button below to import the blueprint

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Ftelegram%2Ftelegram_command.yaml)

A full explanation of all the input fields will be added in the near future

For now here are some examples for custom commands: 

### Custom command to show some system stats.
```yaml
- command: /status
  description: Shows statistics of your Home Assisant instance
  text: >
    🔥 {{ states('sensor.processor_use', with_unit=true) }} CPU load.
    🧠 {{ states('sensor.memory_use', with_unit=true) }} ({{states('sensor.memory_use_percent', with_unit=true) }}) used.
    💽 {{ states('sensor.disk_use', with_unit=true) }} ({{states('sensor.disk_use_percent',with_unit=true) }}) used, still {{ states('sensor.disk_free', with_unit=true) }} free.

    🛢️ The database database is nu {{ states('sensor.database_size_sqlite',with_unit=true) }}.

    🚀 HA has started {{ time_since(states('sensor.home_assistant_uptime') | as_datetime, 2) }} ago, and the host started {{ time_since(states('sensor.last_boot') | as_datetime, 2) }} ago.
        
    💾 The last backup is {{ time_since(states('sensor.backup_last_successful_automatic_backup') | as_datetime) }} ago, and the next one is planned in {{ time_until(states('sensor.backup_next_scheduled_automatic_backup') | as_datetime) }}.
  actions: []
```

Result:

<img alt="telegram alert example image" src="https://github.com/TheFes/ha-blueprints/blob/main/images/telegram_command_status_example.png">

### Custom command to toggle a light 
```yaml
- command: /toggle
  description: Toggle a light in Home Assistant
  text: "{{ args | map('state_attr', 'friendly_name') | join(', ') }} toggled"
  actions:
    - action: light.toggle
      target:
        entity_id: "{{ args }}"
```

Result:

<img alt="telegram alert example image" src="https://github.com/TheFes/ha-blueprints/blob/main/images/telegram_command_toggle_example.png">

⚠️ IMPORTANT: Only actions actually using `action:` are supported. If you need building blocks like `choose` or `delay` or others you can create a script and use that. Use `script.turn_on`  and target the script, to avoid delays or other wait periods affecting the command automation.

***

## ☕ Coffee

If you think I deserve a coffe, please feel free to buy me one (I might spend it on another beverage though).
In case you decide to do so, thanks a lot!

<a href="https://www.buymeacoffee.com/thefes" target="_blank">![Buy Me A Coffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)</a>

Or you can do a small donation using PayPal.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/paypalme/thefes)
