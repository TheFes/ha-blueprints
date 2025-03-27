![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Set reminders

### Prerequisites

* Createa a ToDo entity to store the reminders in. You can use the button below to start the config flow

[![Open your Home Assistant instance and start setting up a new integration.](https://my.home-assistant.io/badges/config_flow_start.svg)](https://my.home-assistant.io/redirect/config_flow_start/?domain=todo)

### Blueprint setup

#### Required

* Set a todo entity to be used to store the reminders

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