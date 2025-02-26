![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Calendar Entries - Local

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fcalendar%2F1_voice_calendar_entries_local.yaml)

Using this blueprint you can create an automation to request the calendar entries. The automation runs totally local, no LLM is required to use this automation.

## Limitations of this automation

* The sentences used to trigger the automation are rather strict
* For full days you can only ask calendar entries up to one weak ahead and for a period of one full day. For partial days only `night`, `morning`, `afternoon` and `evening` are possible, no other (subset of) hours.
* The response is predefined, and therefor will follow the same format every time you request the calendar entries. 
* In case you want to use it in another language as English, you need to provide translations for a large number of settings.

## How to use the blueprint

1. Import the blueprint using the button above
2. Create an automation using the blueprint
3. Provide a calendar entity which will be used for the calendar entries
4. (optionally) change or add the triggers provided in the blueprint settings
5. (optionally) change the phrases used to refer to the days and day parts
6. (optionally) change the response given by Assist

## Detailed instructions for blueprint settings

### Calendar entity

|Setting|Type|Description|
|---|---|---|
|Calendar Entity|Entities|The calendar entity or calendar entities which will be used to get the calendar entries from.|

### Trigger settings

|Setting|Type|Description|
|---|---|---|
|Trigger for calendar entries|Text|The sentences which will trigger the automation. `{phrase}` refers to the moment from which you want the entries, e.g. `Tomorrow` or `afternoon`|

#### Trigger sentence syntax

||Description|Example
|---|---|---|
|`[` `]`|An optional part, the sentence will trigger with, or without it|`what is the [current] weather`|
|`\|`|Indicates `or`. Can also be used in an optional part|`what is the weather [currently\|right now]`|
|`(` `)`|Can be used in combination with the `\|` to indicate at least one of the options is required|`(what's\|what is) the weather`|

Capitalization is ignored when checking a (voice) command with a trigger sentence. Double spaces (due to optional parts) are trimmed.

### Settings for the daily calendar entries / Settings for the calendar entries of a part of a day

These settings can be use to set which terms can be used to request a calendar entries. Multiple terms can be entered for a single parameter.

#### Note on the terms for part of a day

When checking for the terms which are part of the day, they will also be checked against the values set for `Tomorrow`. If there is a match for one of the terms for `Tomorrow`, a day will be added.
So `What is the weather for this morning` will give the weather for the morning of today, and `What is the weather for tomorrow morning` will give the calendar entries for the morning of the next day.

### Settings for the responses

You can change how Assist responds to the voice command here.
