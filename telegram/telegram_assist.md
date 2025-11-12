<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# 💬 Telegram assist blueprint

This blueprint creates an automation which processes all messages sent in a Telegram chat as voice commands. I can process them locally (only) and/or send them to an LLM for processing.

<img alt="telegram alert example image" src="https://github.com/TheFes/ha-blueprints/blob/main/images/telegram_assist_example.jpeg">

***

## ❓ How to use

Import the blueprint using this button:

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Ftelegram%2Ftelegram_assist.yaml)

Then create a new automation using the blueprint and provide the settings.

The text between brackets is the key for the input used in YAML.

* #### **Telegram event entity**  _(telegram_event)_ | no default
  The event entity or entities which belong to the Telegram chats for which the messages should be handled as voice messages.

* #### **LLM conversation entity** _(llm_entity)_  | no default
  The LLM conversation entities to be used as fallback of the local processing.
  They will be processed in order, and the first result which isn't an error will be returned.
  Leave empty to use local handling only.

***

## ☕ Coffee

If you think I deserve a coffe, please feel free to buy me one (I might spend it on another beverage though).
In case you decide to do so, thanks a lot!

<a href="https://www.buymeacoffee.com/thefes" target="_blank">![Buy Me A Coffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)</a>

Or you can do a small donation using PayPal.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/paypalme/thefes)
