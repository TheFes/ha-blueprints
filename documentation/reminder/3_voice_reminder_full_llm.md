![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Set reminders

# 🚨 ONLY WORKS ON 2025.4.0 which is in beta now

### Imporatant

* This blueprint also needs you to set up a ToDo entity to store the reminders in. The script will create the entries in this ToDo list.

[![Open your Home Assistant instance and start setting up a new integration.](https://my.home-assistant.io/badges/config_flow_start.svg)](https://my.home-assistant.io/redirect/config_flow_start/?domain=todo)

* To send out the notifications you need to add an automation. It will trigger every minute and check the ToDo list for items which are (almost) due. You can import the blueprint using the button. Instructions on how to set up the automation blueprint can be found [below](#blueprint-setup-for-automation)

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Freminder%2F0_voice_send_reminder.yaml)

### Blueprint setup for script

#### Optional

* Change the prompt settings for the LLM

* Change or translate the settings for the response given by Assist

#### Note:

* Give the script a clear description. This will be used by the LLM to understand
it should use this script for the todo list items.

* **Make sure to expose the script to Assist after the script has been saved**

#### Example for script description:


`Tool to set reminders based on voice commands. In case not recipient is provided ask for it, 
in case no clear date and time is provided ask for it, in case no recipient is provided ask 
for it`

### Usage

You can request for a reminder to be set. The LLM will ask you for missing data.

#### Examples

```
Set a reminder to take out the trash
Remind me to buy flowers for my partner
Make sure I do not forget to buy a gift for Mothers day
```

### Blueprint setup for automation

#### Required

* Set the LLM conversation agent which will be used to process the recipients to the right names

* Set the ToDo list which is used to store the reminders

* Enter the recipient data, it should contain at least the name and description and one notifier. 

The notifier should be an action to send the notification, it can be any action you want, which supports
the `message` field in the data. So a persistent notification, tts message, notify message, etc.

You can add addtional action data as normal, for example a `title`.

It is also possible to use a jinja template as `condition` to determine if the reminder should be sent, 
however, even if the condition is `false` the reminder will be removed from the todo list, so always 
make sure that at least one notifier returns `true`

Lastly it is possible to add a `prefix` for the message, so for example you can prefix the a TTS announcement 
with `This is a reminder for: `

Here is an example with comments to explain it in more detail. Make sure to adjust this to your own system.

```yaml
- name: Richard # the name of the recipient
  description: Father of the house, also called Dick # decription of the recipient which will be used by the LLM
  notifiers:
    # first notifier action
    - action: notify.mobile_app_dick
      # additional action data
      data:
        data:
          channel: Reminders
          ttl: 0
          priority: high
          notification_icon: mdi:reminder
    # 2nd notifier action, will only be used if Richard is at home
    - action: tts.speak
      target:
        entity_id: tts.home_assistant_cloud
      data:
        media_player_entity_id: media_player.office_dick
      # prefix which will be added to the description of the reminder
      prefix: "This is a reminder for:"
      # condtion to determine if the reminder will be sent
      condition: "{{ is_state('person.richard', 'home') }}"
```

#### Optional

* Adjust the prompt used to get the right names
