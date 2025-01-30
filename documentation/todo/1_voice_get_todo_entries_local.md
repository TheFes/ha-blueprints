![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Get ToDo items Entries - Local

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Ftodo%2F1_voice_get_todo_entries_local.yaml)

Using this blueprint you can create an automation to request the todo entries. The automation runs totally local, no LLM is required to use this automation.

## Limitations of this automation

* The sentences used to trigger the automation are rather strict
* For full days you can only ask calendar entries up to one weak ahead and for a period of one full day. For partial days only `night`, `morning`, `afternoon` and `evening` are possible, no other (subset of) hours.
* The response is predefined, and therefor will follow the same format every time you request the calendar entries. 
* In case you want to use it in another language as English, you need to provide translations for a large number of settings.

## How to use the blueprint

1. Import the blueprint using the button above
2. Create an automation using the blueprint
3. Provide a weather entity which will be used for the calendar entries
4. (optionally) change or add the triggers provided in the blueprint settings
5. (optionally) change the phrases used to refer to the days and day parts
6. (optionally) change the response given by Assist

## Detailed instructions for blueprint settings

### Default entity

|Setting|Type|Description|
|---|---|---|
|Default entity|Entity|The todo entity which will be used if you do not specify a list, or use the hard coded list in the trigger (by default the shopping list).|

### Trigger settings

|Setting|Type|Description|
|---|---|---|
|Trigger for calendar entries|Text|The sentences which will trigger the automation. `{list_nmae}` refers to name of the todo entity|

#### Trigger sentence syntax

||Description|Example
|---|---|---|
|`[` `]`|An optional part, the sentence will trigger with, or without it|`what is the [current] weather`|
|`\|`|Indicates `or`. Can also be used in an optional part|`what is the weather [currently\|right now]`|
|`(` `)`|Can be used in combination with the `\|` to indicate at least one of the options is required|`(what's\|what is) the weather`|

Capitalization is ignored when checking a (voice) command with a trigger sentence. Double spaces (due to optional parts) are trimmed.

### Settings for the responses

You can change how Assist responds to the voice command here.
