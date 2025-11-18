<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

In this folder you will find 3 blueprints which can be used with the [Telegram bot](<https://www.home-assistant.io/integrations/telegram_bot>) integration.

The blueprints require at least Home Assistant 2025.11 as it uses the event entities for Telegram bots introduced in that version.
It also requires your Telegram bot to either use polling or webhooks. In case polling is used, there will be delay in processing the results.


# 🚨 Telegram alert blueprint

This blueprint is intended as a replacement for the [alert](<https://www.home-assistant.io/integrations/alert/>) integration and uses Telegram to send the alert messages.

For more information see the [blueprint documentation](/telegram/telegram_alert.md)

### 👇 Example:
<img alt="telegram alert example image" src="https://github.com/TheFes/ha-blueprints/blob/main/images/telegram_bot_example_image.jpeg">


# 💬 Telegram assist blueprint

This blueprint creates an automation which processes all messages sent in a Telegram chat as voice commands. I can process them locally (only) and/or send them to an LLM for processing.

For more information, see the [blueprint documentation](/telegram/telegram_assist.md)

### 👇 Example:
<img alt="telegram alert example image" src="https://github.com/TheFes/ha-blueprints/blob/main/images/telegram_assist_example.jpeg">


# 💬 Telegram command blueprint

This blueprint creates an automation to use custom commands in Telegram, including a `/help` command to show all available commands, and commands to restart and update Home Assistant components. By default a `/restart` and `/update` command are also made available.

### 👇 Example for a custom `/status` command:
<img alt="telegram alert example image" src="https://github.com/TheFes/ha-blueprints/blob/main/images/telegram_command_status_example.png">

For more information, see the [blueprint documentation](<https://github.com/TheFes/ha-blueprints/blob/main>)