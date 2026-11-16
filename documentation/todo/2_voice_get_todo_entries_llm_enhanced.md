![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Get ToDo items - LLM Enhanced

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Ftodo%2F2_voice_get_todo_entries_llm_enhanced.yaml)

Using this blueprint you can create an automation to ask questions about your todo lists, like the shopping list. The question is caught by local trigger sentences, so the voice pipeline can use the local Home Assistant conversation agent (or an LLM with local preference), and the LLM doesn't need access to your house.

An [AI Task](https://www.home-assistant.io/integrations/ai_task/) (`ai_task.generate_data`) is used twice:

1. To determine which list(s) you mean, and whether you want the open or completed items. This uses structured output with the names of your lists, so the AI can only return lists which exist.
2. To answer your question, based on the items on those lists. The answer is in the language of the question.

## Requirements

* Home Assistant 2026.11 or newer (the blueprint uses `entity_name` for the list names)
* An AI Task entity. You can select it in the blueprint, or set a preferred AI Task entity in Home Assistant

## Limitations of this automation

* The sentence still has to match one of the trigger sentences. Within those sentences you can ask anything
* The trigger sentences and the error response need to be translated when you use another language than English
* Each question uses two AI Task calls, so the response takes a bit longer than with the local blueprint
* The list names are sent to the AI Task. Restrict the lists with the `Todo lists` setting when there are lists the AI shouldn't know about

## Blueprint settings

|Setting|Type|Description|
|---|---|---|
|Todo lists|Entities|The todo lists which can be used. Leave empty to use all todo lists|
|Default list|Entity|The list used when the question doesn't mention a list (for example the shopping list). Leave empty to use all lists|
|AI Task entity|Entity|The AI Task entity to use. Leave empty to use the preferred AI Task entity|
|Instructions to determine the lists|Text|How the AI should determine the lists. `{{ trigger.sentence }}` is the question, `{{ default_list_name }}` the default list|
|Instructions for the response|Text|How the AI should answer. The question and the items are added automatically|
|Conversation trigger sentences|Text|The sentences which trigger the automation. `{query}` is a wildcard, the full sentence is always sent to the AI|
|Error response|Text|Used when the AI Task fails, or the items could not be retrieved|

## What the AI receives

The requested status, the current time, the number of items, and per list: the name of the list, the number of items and the items themselves (at most 100 per list). Each item has:

|Key|Description|
|---|---|
|`summary`|The item (line breaks and double spaces removed)|
|`status`|Only when both open and completed items are requested|
|`due`, `due_day`|The due date, and the day in words, for example `Tuesday 18 February 2025`|
|`overdue`|Only present (and `true`) for open items which are past their due date|
|`description`|When set. Cut off after 200 characters|

Only open items are sent, unless the question is about completed items.

## Choosing a model

The AI Task entity can be a small, fast model. In testing, `gemini-3.1-flash-lite` sometimes answered an English question in Dutch when the items on the list were in Dutch. When you notice things like that, select a slightly larger model for the AI Task.

## Translation

### Dutch

```yaml
trigger:
  - "wat staat er op [de|mijn] {query}"
  - "(heb|hebben) (ik|we) [nog] {query} nodig"
  - "staat {query} op [de|mijn] [boodschappen]lijst"
  - "wat (moet|moeten) (ik|we) [nog] (kopen|halen|doen) [{query}]"
error_response: Sorry, er ging iets mis bij het ophalen van je lijstjes
```

## Examples

These are real answers from testing, with a Dutch shopping list:

- *Do we need ginger*

  *Ja, gember staat op de boodschappenlijst.* (the answer was in Dutch, see [Choosing a model](#choosing-a-model))

- *Hebben we nog melk nodig*

  *Nee, er staat geen melk op je boodschappenlijst.*

- *Which tasks are overdue*

  *The only overdue task on the Testerdetest list is Test 1, which was due on February 18, 2025.*

- *Wat staat er op al mijn lijstjes*

  *Op je lijst Home Assistant staan ik ben thuis script maken en dit is een test. Op je boodschappenlijst staan ... De lijst Reminder is leeg.*
