<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# 💬 Telegram command blueprint

This blueprint creates an automation to use custom commands in Telegram.

By default the following commands are added:

`/help`: This will display all available commands with a description. The descriptions can be provided and/or adjusted in the blueprint settings. The example below shows two custom commands added using the blueprint (`/status`  and `/toggle`). These examples are shown in the [Custom commands settings](#️-custom-command-settings) section.

<img alt="telegram alert example image" src="https://github.com/TheFes/ha-blueprints/blob/main/images/telegram_command_help_example.png">

`/restart`: This command will show Home Assistant components which can be restarted. By default an option to restart Home Assistant Core is added. For each component an argument is provided (for Home Assistant Core this is 
`core`) which can be added as argument to prevent the first message asking which component to restart. For example `/restart core` will immediately indicate that you want to restart the Home Assistant Core component. Optionally you can let the bot ask for confirmation before the restart command is issued. This is enabled by default. In case no components to restart are provided, so when the default Home Assistant Core option is removed, and no others are added, the command is disabled.

<img alt="telegram alert example image" src="https://github.com/TheFes/ha-blueprints/blob/main/images/telegram_command_restart_example.png">

`/update`: This command will list all pending updates in Home Assistant, and for each of them show some details and buttons to either install or skip the update. You can optionally provide entities or integrations which should not be used for this command. All related text can optionally be amended or translated.

<img alt="telegram alert example image" src="https://github.com/TheFes/ha-blueprints/blob/main/images/telegram_command_update_example.png">

There is also an option to provide custom commands. By default the `/ping` command is added, which will simply return `Pong 🏓` and can be used to test if the automation is working.
All custom commands can return a message, and or perform actions. You can find examples in the [Custom commands setting](#️-custom-command-settings) section.

***

## ❓How to use

Use the button below to import the blueprint

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Ftelegram%2Ftelegram_command.yaml)


Then create a new automation using the blueprint. The settings are divided into separate sections. For every setting where there is no default provided, you'll need to provide input. Note that the `Restart button texts` and `Update button texts` are collapsed by default.

The text between brackets is the key for the input used in YAML.

### 💬 <u>Chat settings</u>

* #### **Telegram event entity**  _(event_entity)_ | no default
  The event entity or entities from the Telegram Bot integration which referencing the chat in which you want to give the commands.

### ⚙️ <u>Generic settings</u>

* #### **Parse mode** _(parse_mode)_ | default: `markdown`
  This will determine the formatting of the message. By default `markdown` formatting is used, but you can also select `markdownv2`, `html` and `plain_text`. Note that `markdownv2` needs escaping of special characters and will otherwise result in an error.

* #### **Disable web page preview** _(disable_web_page_preview)_ | default: `true`
  When disabled the alert message will not display previews of web pages in case an website url is sent in the message.

* #### **Press timeout** _(pres_timeout)_ | default: `10`
  The number of minutes which the automation will wait for button presses in the `/restart` and `/update` commands

* #### **Timeout message** _(timeout_message)_ | default: `"Button press timeout for {{ command }} exceeded. The command is stopped."`
  Text to be shown after the wait period for a button press is over. You can use Jinja templates in it, the `command` variable can be used to refer to the command for which the automation was waiting.

### 🧩 <u>Custom command settings</u>

* #### **Custom commands** _(custom_commands)_ | default: `/ping` _(see below)_

  Here you can provide custom commands for your Telegram bot. Each command consisists of 4 parts
  - `command`: The command which can be used in the bot
  - description: The description which will be shown in the `/help` command for this specific command
  - `text`: The text that will be returned in the chat after issuing the command
  - `actions`: The actions which will be performed after issuing the command.
    ⚠️ **IMPORTANT**: Only actions actually using `action:` are supported. If you need building blocks like `choose` or `delay` or others you can create a script and use that. Use `script.turn_on`  and target the script, to avoid delays or other wait periods affecting the command automation.

  By default the `/ping` command is added. If issued it will simply return `Pong 🏓` and it can be used to test if the automation is working.

  The settings for this command are:
  ```yaml
  - command: /ping
    description: Pong!
    text: Pong 🏓
    actions: []
  ```

  Some other examples: 👇

  #### Custom command to show some system stats.
  This example uses the [System Monitor](<https://www.home-assistant.io/integrations/systemmonitor/>) integration and the Backup integration to retrieve some statistics, and then sends that to the bot in a message.

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

  #### Custom command to toggle a light
  This command uses the argument provided together with the command, and toggles that specific light. As shown below you can use the `args` variable to refer to these arguments. Note that this will be a list, also when only one argument is provided.

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

### 🔁 <u>Restart settings</u>

* #### **Restart** _(restart)_ | default: `core` _(see below)_

  This part works similar as for the custom commands, with an object selector in which you define three parts:
  - `name`: The name to be displayed on the actionable button and in the responses by the Telegram bot
  - `arg` The argument which can be used to restart this specific component without the need to select it using a button.
  - `actions`: The actions which will be performed when the button is pressed, after an optional additional confirmation.

  By default the settings to restart Home Assistant Core are added, which look like this:
  ```yaml
  - name: Home Assistant core
    arg: core
    actions:
      - action: homeassistant.restart
        data: {}
  ```

  So you can use this directly when you issue the `/restart core` command.

 * #### **Ask verification** _(ask_verification)_ | default: `true` 
   Wether to ask for verification before performing the restart. This can be bypassed by adding the `yes` argument with your command.
   So the built in Home Assistant Core restart can be performed without further messages by issuing `/restart core yes`

### 🗒️ <u>Restart button texts</u>

All texts used messages regarding the `/restart` command can be adjusted here. You can also use this to translate the default English texts to another language.
The `restart_text` input field support Jinja templates. You can use `restart_name` variable to refer to the name used for the component.

### 🆙 <u>Update settings</u>

 * #### **Use udpdate** _(use_update)_ | default: `true` 
   Disable if you don't want to use the `/update` command to update any pending update entity

 * #### **Ignore entities** _(ignore_entities)_ | default: `[]` 
   Entities which can not be updated using the `/update` command

 * #### **Ignore integrations** _(ignore_integrations)_ | default: `[]` 
   Integrations which should be ignored when using the `/update` command. Provide the entities as a YAML list. You can either use the intergration id, or the config entry title. 

   Example:
   ```yaml
   - esphome
   - Hue bridge downstairs
   ```

### 🗒️ <u>Update button texts</u>

All texts used messages regarding the `/update` command can be adjusted here. You can also use this to translate the default English texts to another language.
Several fields support Jinja templates. In these fields you can use the `update_name`, `installed_version`, `latest_version` and `release_url` variables. The value for `release_url`  will be `none` in case the update entity doesn't provide it.

***

## ☕ Coffee

If you think I deserve a coffe, please feel free to buy me one (I might spend it on another beverage though).
In case you decide to do so, thanks a lot!

<a href="https://www.buymeacoffee.com/thefes" target="_blank">![Buy Me A Coffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)</a>

Or you can do a small donation using PayPal.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/paypalme/thefes)
