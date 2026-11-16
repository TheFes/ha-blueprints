<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# Get ToDo items - Local

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Ftodo%2F1_voice_get_todo_entries_local.yaml)

Using this blueprint you can create an automation to request the todo entries. The automation runs totally local, no LLM is required to use this automation.

## Requirements

* Home Assistant 2024.10 or newer

## Limitations of this automation

* The sentences used to trigger the automation are rather strict
* Only the open items are listed, and the response is predefined, so it will follow the same format every time
* The list name is matched with the names of the todo entities, aliases are not supported
* In case you want to use it in another language as English, you need to translate the trigger and the responses. You can find translations in the `translations` folder in this directory.

## How to use the blueprint

1. Import the blueprint using the button above
2. Create an automation using the blueprint
3. Select the default todo list (for example your shopping list)
4. (optionally) change or add the triggers provided in the blueprint settings
5. (optionally) change the response given by Assist

## Detailed instructions for blueprint settings

### Default entity

|Setting|Type|Description|
|---|---|---|
|Default entity|Entity|The todo entity which will be used if you do not specify a list, or use the hard coded list in the trigger (by default the shopping list).|

### Trigger settings

|Setting|Type|Description|
|---|---|---|
|Trigger for Todo list items|Text|The sentences which will trigger the automation. `{list_name}` refers to the name of the todo list. It is matched (not case sensitive) first on the complete name, then on a part of the name. Without `{list_name}` the default list is used|

#### Trigger sentence syntax

||Description|Example
|---|---|---|
|`[` `]`|An optional part, the sentence will trigger with, or without it|`what is the [current] weather`|
|`\|`|Indicates `or`. Can also be used in an optional part|`what is the weather [currently\|right now]`|
|`(` `)`|Can be used in combination with the `\|` to indicate at least one of the options is required|`(what's\|what is) the weather`|

Capitalization is ignored when checking a (voice) command with a trigger sentence. Double spaces (due to optional parts) are trimmed.

### Settings for the responses

You can change how Assist responds to the voice command here.

|Setting|Description|
|---|---|
|No items on selected list|Response when the list is empty. `{list_name}` is replaced|
|Invalid list name|Response when no list matches the requested name. `{list_name}` is replaced with the requested name|
|Response in case there are items|`{item_count}`, `{list_items}` (one item per line) and `{list_name}` are replaced|
