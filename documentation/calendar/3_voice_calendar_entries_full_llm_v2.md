<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# Calendar entries - Full LLM - version 2

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fcalendar%2F3_voice_calendar_entries_full_llm_v2.yaml)

Version 2 of the [Full LLM script](3_voice_calendar_entries_full_llm.md). The LLM in your voice pipeline only passes the question in a single `query` field. An [AI Task](https://www.home-assistant.io/integrations/ai_task/) (`ai_task.generate_data`) with structured output determines the requested period, Home Assistant collects the events of the selected calendars, and the result is returned to the LLM, which answers the question.

It is added next to version 1, because the fields of the script changed. Version 1 keeps working.

## Requirements

* Home Assistant 2026.11 or newer (the blueprint uses `entity_name` for the calendar names)
* An LLM conversation agent in your voice pipeline which is allowed to control your house (so it can call the script)
* An AI Task entity. This can be the same integration as your conversation agent, but you can also use a smaller and faster model

## How to use the blueprint

1. Import the blueprint using the button above
2. Create a script using the blueprint
3. Select the calendar entities, and optionally the AI Task entity (leave empty to use the preferred AI Task entity)
4. Give the script a clear description (example below)
5. Save the script, and expose it to Assist

### Example for script description:

> Gets the events from my calendars. Use this tool for every question about appointments, events or what is planned. Pass the complete question of the user in the query field, in the words of the user.

## Blueprint settings

|Setting|Type|Description|
|---|---|---|
|Calendar entities|Entities|The calendars to get the events from|
|AI Task entity|Entity|The AI Task entity used to determine the period. Leave empty to use the preferred AI Task entity|
|Instructions to determine the period|Text|How the AI Task should translate the question into a period. `{{ query }}` is the question|
|Query field description|Text|The description of the `query` field, which tells the LLM what to pass to the script|

## What the LLM receives

For the requested period: the start and end, the current time, the number of events and the events themselves (at most 100). Each event has:

|Key|Description|
|---|---|
|`summary`|The title of the event (line breaks and double spaces removed)|
|`start`, `end`|Start and end, a date for all-day events|
|`day`|The day of the event in words, for example `Monday 23 November 2026`, so the AI doesn't have to work out the weekday itself|
|`all_day`|`true` for all-day events|
|`calendar`|The name of the calendar|
|`already_ended`|Only present (and `true`) when the event has already ended, so "when is my next ..." isn't answered with an appointment from this morning|
|`location`, `description`|When set. The description is cut off after 200 characters|

When no period is mentioned in a question like "when is my next dentist appointment", the period is today plus the coming 60 days.

When something goes wrong (the AI Task fails, or the calendars can't be read) the LLM receives an `error`.

## What is different compared to version 1

|Topic|Version 1|Version 2|
|---|---|---|
|Fields|Four fields (`time_period_type`, `time_period_length`, `start_date`, `start_time`), the LLM has to work out the dates and times itself|One field: `query`. The LLM only passes the question|
|Determining the period|By the LLM in your voice pipeline, guided by four prompt inputs|By an AI Task with structured output, which gets the current date and time explicitly. One set of instructions replaces the four prompts|
|Events|As returned by the calendars|With the calendar name, the day in words, `already_ended` for events in the past, cleaned up titles and shortened descriptions|
|"When is my next ..."|Depends on the period the LLM chooses|Today plus the coming 60 days|

### Removed and renamed inputs

|Version 1|Version 2|
|---|---|
|`time_period_type_prompt`, `time_period_length_prompt`, `date_prompt`, `time_prompt`|`llm_instructions` (for the AI Task) and `query_prompt` (the description of the `query` field)|
|`calendar_entities`|Unchanged|
|-|New: `ai_task_entity`|

### Switching from version 1

Create a new script from the version 2 blueprint (or change the blueprint of your existing script), give it the description above, and make sure only one of them is exposed to Assist. The LLM can't know which one to use when both are exposed.
