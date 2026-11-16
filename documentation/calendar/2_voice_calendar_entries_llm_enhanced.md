![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Calendar Entries - LLM Enhanced

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fcalendar%2F2_voice_calendar_entries_llm_enhanced.yaml)

Using this blueprint you can create an automation to ask questions about your calendar. The question is caught by local trigger sentences, so the voice pipeline can use the local Home Assistant conversation agent (or an LLM with local preference), and the LLM doesn't need access to your house.

An [AI Task](https://www.home-assistant.io/integrations/ai_task/) (`ai_task.generate_data`) is used twice:

1. To determine the requested period from your question. This uses structured output, so the AI can only return a period type, a date, a time and a length.
2. To answer your question, based on the events in that period. The answer is in the language of the question.

## Requirements

* Home Assistant 2026.11 or newer (the blueprint uses `entity_name` for the calendar names)
* An AI Task entity. You can select it in the blueprint, or set a preferred AI Task entity in Home Assistant
* One or more calendar entities

## Limitations of this automation

* The sentence still has to match one of the trigger sentences. Within those sentences you can ask anything
* The trigger sentences and the error response need to be translated when you use another language than English
* Each question uses two AI Task calls, so the response takes a bit longer than with the local blueprint

## How to use the blueprint

1. Import the blueprint using the button above
2. Create an automation using the blueprint
3. Select the calendar entities
4. (optionally) select the AI Task entity, if you don't want to use the preferred one
5. (optionally) change or translate the trigger sentences
6. (optionally) adjust the instructions for the AI

## Blueprint settings

|Setting|Type|Description|
|---|---|---|
|Calendar entities|Entities|The calendars to get the events from|
|AI Task entity|Entity|The AI Task entity to use. Leave empty to use the preferred AI Task entity|
|Instructions to determine the period|Text|How the AI should translate the question into a period. `{{ trigger.sentence }}` is the question|
|Instructions for the response|Text|How the AI should answer. The question, the period and the events are added automatically. `{{ trigger.sentence }}` and `{{ period_description }}` can be used|
|Conversation trigger sentences|Text|The sentences which trigger the automation. `{query}` is a wildcard, the full sentence is always sent to the AI|
|Error response|Text|Used when the AI Task fails, or the calendar events could not be retrieved|

## What the AI receives

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

## Choosing a model

The AI Task entity can be a small, fast model, but very small models can make mistakes when summarizing many events. In testing, `gemini-3.1-flash-lite` sometimes answered an English question in Dutch when the event titles were in Dutch. When you notice things like that, select a slightly larger model for the AI Task.

## Translation

### Dutch

```yaml
trigger:
  - "wat staat er [{query}] op [de|mijn] (agenda|kalender|planning)"
  - "wat (heb ik|staat er) {query} gepland"
  - "ben ik {query} (vrij|bezet|beschikbaar)"
  - "wanneer (is|heb ik) [mijn] [volgende] {query}"
error_response: Sorry, er ging iets mis bij het ophalen van je afspraken
```

## Examples

These are real answers from testing:

- *Am I free on Thursday afternoon*

  *You are not free on Thursday afternoon. You have an all day event for the Home Assistant hackathon in Den Haag, and you also have an appointment to pick up cheese and vegetables around half past twelve.*

- *What do I have planned this weekend*

  *You have four things planned this weekend. On Saturday, November 21, you have Kattenbak at nine in the morning, Sinterklaas werk Martijn at one thirty in the afternoon, and a Biertje in buitenbeentje at nine in the evening. On Sunday, November 22, you are going to the Schouwburg ...*

- *Wanneer ga ik weer naar de fysio*

  *Ik kan in je agenda geen toekomstige afspraken voor de fysio vinden. Je bent vandaag al geweest.*

## Differences with the other calendar blueprints

|Topic|1: Local|2: LLM Enhanced|3: Full LLM script|
|---|---|---|---|
|LLM required|No|AI Task only|LLM conversation agent with access to your house|
|Sentences|Strict trigger sentences, with fixed phrases for the period|Trigger sentences, the period can be anything|Anything|
|Periods|A day up to a week ahead, this week, the weekend, next week, or night, morning, afternoon, evening (optionally with a day)|Any period, hours or days, up to a year|Any period|
|Questions about specific events|No|Yes ("when is my next dentist appointment")|Yes|
|Response|Fixed template|Generated by the AI, in the language of the question|Generated by the LLM|
