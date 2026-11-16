<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# Add Calendar Event - LLM Enhanced

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fcalendar%2F2_voice_add_calendar_event_llm_enhanced.yaml)

Using this blueprint you can create an automation to add events to your calendars. The request is caught by local trigger sentences, so the voice pipeline can use the local Home Assistant conversation agent (or an LLM with local preference), and the LLM doesn't need access to your house.

An [AI Task](https://www.home-assistant.io/integrations/ai_task/) call (`ai_task.generate_data`) with structured output determines the title, the calendar (a select, so only existing calendars can be returned), the date, the start and end time, optionally the location and a description, and a question for missing information or a confirmation, both in the language of the request.

## Follow-up questions

* **On a voice satellite**: when the title or the date is missing, Assist gives no response, and a second run of the automation (started with the `voice_calendar_event_follow_up` event) waits until the satellite is idle and asks for it with `assist_satellite.ask_question`. This repeats until everything is known, or the maximum number of questions is reached. The confirmation is announced on the satellite
* **In text chat** (or with the maximum number of questions set to 0): the response says what is missing, and you need to say the complete request again

## How the event is added

* Without a time an **all-day event** is added. An all-day event can last more than one day ("from Friday to Sunday")
* With a start time but without an end time or duration, the **default duration** (60 minutes) is used
* When only a time is mentioned, today is used, or tomorrow when that time has already passed
* Only calendars which support adding events can be used (for example Local Calendar, Google Calendar and CalDAV). Calendars of other integrations are skipped

## Requirements

* Home Assistant 2025.7 or newer
* An AI Task entity. You can select it in the blueprint, or set a preferred AI Task entity in Home Assistant

## Limitations

* The sentence has to match one of the trigger sentences
* The default sentence `(add|put) {query} (to|on|in) [the|my] [{calendar}] (calendar|agenda) [{due}]` looks a lot like the sentence of the [Add ToDo item with due date](../todo/2_voice_add_todo_item_llm_enhanced.md) blueprints. When you use both, only one of them may handle a sentence, so change the trigger sentences of one of them
* Follow-up questions only work on Assist satellites which support `ask_question` (like the Home Assistant Voice Preview Edition)

## Blueprint settings

|Setting|Description|
|---|---|
|Calendars|The calendars events can be added to. Leave empty to allow all calendars which support adding events|
|Default calendar|The calendar used when no calendar is mentioned|
|Default duration|The duration in minutes when only a start time is mentioned, default 60|
|Trigger sentences|The complete sentence is passed to the AI. Default: `(add\|put) {query} (to\|on\|in) [the\|my] [{calendar}] (calendar\|agenda) [{due}]` and `(schedule\|plan) {query}`|
|Maximum number of follow-up questions|Default 2, set to 0 to never ask|
|AI Task entity|Leave empty to use the preferred AI Task entity|
|Instructions for the AI|`{{ conversation }}` is the request (and the follow-up questions and answers), `{{ default_calendar_name }}` the default calendar|
|Information missing|Used in text chat, or when no more questions may be asked. `{question}` is the question for the missing information|
|No answer on the follow-up question|Announced when the satellite didn't hear an answer|
|Error response|Used when the AI Task fails, or the event could not be added|

## Translation

### Dutch

```yaml
trigger:
  - "(zet|doe) {query} (in|op) [de|mijn] [{calendar}] (agenda|kalender) [{due}]"
  - "plan {query}"
  - "maak een afspraak {query}"
response_missing: "{question} Zeg je hele verzoek nog een keer, inclusief dit."
response_no_answer: Ik heb geen antwoord gehoord, dus ik heb de afspraak niet toegevoegd
response_error: Sorry, het is niet gelukt om de afspraak toe te voegen
```
