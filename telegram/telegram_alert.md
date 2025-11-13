<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# 🚨 Telegram alert blueprint

This blueprint is intended as a replacement for the [alert](<https://www.home-assistant.io/integrations/alert/>) integration and uses Telegram to send the alert messages.

### 🫶 It supports these features of the alert integration:
- Start alert when the state of an entity goes to a certain problem state
- Provide a fixed number of minutes the alert should be repeated, or a list of numbers for a variable interval
- Optionally skip the first message, .so it won’t be sent immediately when the entity changes to the problem state
- Provide an option to acknowledge an alert before the entity is no longer in the problem state
- Send a message when the entity changes state, to indicate it is no longer in the problem state

### 🦾 In addition, it also supports:
- Trigger alerts based on an attribute value instead of the entity state
- Add actionable buttons which will perform assigned actions
- Optionally automatically remove previous messages for the alert when a new message is sent
- Optionally automatically remove previous messages for the alert when the alert is done (either the entity state changed or the alert is acknowledged)

### ⚠️ Known limitations:
- On restart of Home Assistant or when modifying the automation for a specific alert, the repeat sequence will restart. This includes resetting any wait periods and starting at the beginning of the repeat list if a list is provided
- After restarting Home Assistant or modifying the automation, messages may be sent for alerts previously acknowledged but still in the problem state
- After restarting Home Assistant, automations will no longer clean up previous alert messages that were active before the restart.

_Note: these limitation can be addressed by saving the alert data in a trigger based template sensor. So I've included support for this, and created a blueprint to create this template sensor.
If you want to use this template sensor, first create it using the [instructions](/other/alert_data_sensor.md). After creation you can select it in the `Alert sensor settings` in the blueprint._

### 👇 Example:
<img alt="telegram alert example image" src="https://github.com/TheFes/ha-blueprints/blob/main/images/telegram_bot_example_image.jpeg">

***

## ❓ How to use

Import the blueprint using this button:

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Ftelegram%2Ftelegram_alert.yaml)

Then create a new automation using the blueprint. The settings are divided into separate sections. For every setting where there is no default provided, you'll need to provide input. Note that the `Acknowledge settings`, `Message cleanup settings` and `Actionable buttons settings` are collapsed by default.

The text between brackets is the key for the input used in YAML.

### 🏴 <u>Trigger settings</u>

* #### **Trigger entity**  _(trigger_entity)_ | no default
  The entity which state will be monitored to start the alert. 


* #### **Problem state** _(problem_state)_  | no default
  The state the trigger entity needs to change to to start the alert. Note that if the entity is already in this state when the automation is created, the alert will not start, it has to change to the state to start the alert.

* #### **Attribute name** _(attribute_key)_  | default: `""`
  You can optionally provide an attribute name so the alert will not trigger on the state of the entity, but on an attribute value. It will use the problem state provided, in combination with the attribute name. This has to be the attribute as shown in developer tools > states, do not use the value from e.g. a more-info card, as they make changes to the name (for example the first character is capitalized).
  When left empty, the state of the entity will be used.

### 🔁 <u>Repeat settings</u>

* #### **Repeat** _(repeat)_ | no default
  The number of minutes to wait between alert messages. You can provide a single number or a list of numbers

  Example single number:
  ```
  15
  ```

  Example list of numbers:
  ```
  - 15
  - 30
  - 60
  ```

* #### **Skip first** _(skip_first)_ | default: `false`
  Controls if the first alert message should be sent immediately when the entity changes to the problem state, or after the first wait period.

### 💬 <u>Message settings</u>

* #### **Telegram bot config entry** _(config_entry)_ | default: `none`
  The config entry of the telegram bot to use, this is not required if you only have one Telegram bot configured.

* #### **Target** _(target)_ | no default
  The target chats to which the alerts should be sent. The selector is using the event entities as created by the Telegram bot integration. These entities have the chat id of the Telegram chat as an attribute, so in the end those chat id's are used as target of the alert messages.

* #### **Parse mode** _(parse_mode)_ | default: `markdown`
  This will determine the formatting of the message. By default `markdown` formatting is used, but you can also select `markdownv2`, `html` and `plain_text`. Note that `markdownv2` needs escaping of special characters and will otherwise result in an error.

* #### **Alert title** _(alert_title)_ | no default
  The title for the alert. This will be the first line of the alert messages sent to the telegram chat(s). Jinja templates are allowed, but you can also use plain text.

* #### **Alert message** _(alert_message)_ | no default
  The message which is sent on every repeat of the alert message. Jinja templates are allowed, but you can also use plain text.

* #### **Disable web page preview** _(disable_web_page_preview)_ | default: `false`
  When disabled the alert message will not display previews of web pages in case an website url is sent in the message.

### ✅ <u>Done message settings</u>

* #### **Done message** _(done_message)_ | default: `""`
  The message which will be sent when the entity is no longer in the problem state. Leave empty to not send a done message.

* #### **Use alert title on done message** _(use_done_title)_ | default: `true`
  By default the alert title will be used as first line for the done message. You can use this setting to turn that off.

### ☑️ <u>Acknowledge settings</u>

* #### **Can acknowledge** _(can_acknowledge)_ | default: `false`
  When enabled it shows a button below the alert message to turn the alert off until it's triggered again.

* #### **Acknowledge button label** _(acknowledge_button_label)_ | default: `"Acknowledge alert"`
  The text to be shown on the button to acknowledge the alert. For example `"Stop bugging me"`  

* #### **Acknowledge message** _(acknowledge_message)_ | default: `""`
  The message which will be sent when the acknowledge button is pressed in the chat. Leave empty to not send a message.

* #### **Use alert title on acknowledgement** _(use_ack_title)_ | default: `true`
  By default the alert title will be used as first line for the acknowledgement message. You can use this setting to turn that off.

### 🧹 <u>Message cleanup settings</u>

* #### **Remove previous message** _(remove_previous_message)_ | default: `true`
  By default the previous alert message will be removed in the chat when a new alert message is sent. You can use this setting to turn that off.

* #### **Remove when done** _(remove_when_done)_ | default: `true`
  By default the previous alert messages will be removed in the chat when the entity is no longer in the problem state or when the alert is acknowledged. You can use this setting to turn that off.

### 📋 <u>Actionable button settings</u>
You can optionally add buttons and assign actions to those buttons.
If you for example have an alert when a light is on, you can add a button to turn that light off.
You can add a maximum of 5 buttons. Below you see the description of button 1, besides the number the settings are the same for the other 4 buttons.

* #### **Button 1 label** _(button_1_label)_ | default: `""`
  The text to be shown on the button. Leave empty to not use this button.

* #### **Button 1 actions** _(button_1_actions)_ | default: `[]`
  The actions to be performed when the button is pressed. When these actions cause the entity to no longer be in the problem state, the alert will be ended, and the done message will be sent (if set).

### 🤖 <u>Alert data settings</u>

* #### **Alert data sensor** _(alert_data_sensor)_ | default: `none`
  To ensure the alert will continue as it should after a HA restart, you can set up an alert data sensor. More instructions [here](/other/alert_data_sensor.md).

* #### **Alert data sensor trigger event** _(alert_event)_ | default: `"update_thefes_alert_sensor"`
  The manual event type on which the alert data sensor triggers.
  In case you didn't change this while setting up the alert data sensor, you can just keep the default setting.

***

## ☕ Coffee

If you think I deserve a coffe, please feel free to buy me one (I might spend it on another beverage though).
In case you decide to do so, thanks a lot!

<a href="https://www.buymeacoffee.com/thefes" target="_blank">![Buy Me A Coffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)</a>

Or you can do a small donation using PayPal.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/paypalme/thefes)
