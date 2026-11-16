<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# Add Calendar Event - Full LLM

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fcalendar%2F3_voice_add_calendar_event_full_llm.yaml)

Using this blueprint you can create a script which can be used as a tool by an LLM conversation agent to add events to your calendars.

The LLM only passes the request in a single `query` field. An [AI Task](https://www.home-assistant.io/integrations/ai_task/) call (`ai_task.generate_data`) with structured output determines the title, the calendar (a select, so only existing calendars can be returned), the date, the start and end time, and optionally the location and a description.

## Follow-up questions

The AI Task is told never to guess. When the title or the date is missing, nothing is added, and the script returns what is missing:

```json
{"success": false, "missing": ["date"], "instruction": "The event was NOT added. Ask the user for the missing information, then call this tool again with the complete request including the answer."}
```

The LLM then asks you for it, and calls the script again with the complete request. When the event is added the script returns `success`, the calendar, the title, the start and the end (for all-day events the last day), so the LLM can confirm it.

## How the event is added

* Without a time an **all-day event** is added. An all-day event can last more than one day ("from Friday to Sunday")
* With a start time but without an end time or duration, the **default duration** (60 minutes) is used
* When only a time is mentioned, today is used, or tomorrow when that time has already passed
* Only calendars which support adding events can be used (for example Local Calendar, Google Calendar and CalDAV). Calendars of other integrations are skipped

## Requirements

* Home Assistant 2025.7 or newer
* An AI Task entity. You can select it in the blueprint, or set a preferred AI Task entity in Home Assistant

## Blueprint setup

* Create the script using the blueprint, and select the default calendar
* Give the script a clear description, the LLM uses it to decide when to use the script. For example:

  `Adds an event or appointment to a calendar. Use this tool whenever the user wants to add, plan or schedule something in a calendar or agenda. Pass the complete request of the user in the query field, in the words of the user. When the tool reports missing information, ask the user for it and call the tool again with the complete request including the answer.`

* **Expose the script to Assist**

## Blueprint settings

|Setting|Description|
|---|---|
|Calendars|The calendars events can be added to. Leave empty to allow all calendars which support adding events|
|Default calendar|The calendar used when no calendar is mentioned|
|Default duration|The duration in minutes when only a start time is mentioned, default 60|
|AI Task entity|Leave empty to use the preferred AI Task entity|
|Instructions for the AI|How the AI should interpret the request. `{{ query }}` is the request, `{{ default_calendar_name }}` the default calendar|
|Query field description|Tells the LLM what to pass to the script|
