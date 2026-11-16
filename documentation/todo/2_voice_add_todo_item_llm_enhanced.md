![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Add ToDo item with due date - LLM Enhanced

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Ftodo%2F2_voice_add_todo_item_llm_enhanced.yaml)

Using this blueprint you can create an automation to add one or more items to a todo list, with a due date and time. The request is caught by local trigger sentences, so the voice pipeline can use the local Home Assistant conversation agent (or an LLM with local preference), and the LLM doesn't need access to your house.

One [AI Task](https://www.home-assistant.io/integrations/ai_task/) call (`ai_task.generate_data`) with structured output determines the items, the list, the due date and time, an optional description and a confirmation in the language of the request.

## Requirements

* Home Assistant 2026.11 or newer (the blueprint uses `entity_name` for the list names)
* An AI Task entity. You can select it in the blueprint, or set a preferred AI Task entity in Home Assistant

## Limitations of this automation

* The sentence still has to match one of the trigger sentences
* The default sentence `(add|put) {item} (on|to) [the|my] {list}` also matches requests without a due moment, so for those sentences this automation is used instead of the built-in intent (which is fine, but uses an AI Task call)
* The trigger sentences and the error response need to be translated when you use another language than English

## Blueprint settings

|Setting|Type|Description|
|---|---|---|
|Todo lists|Entities|The lists items can be added to. Leave empty to allow all todo lists|
|Default list|Entity|The list used when the request doesn't mention a list|
|AI Task entity|Entity|The AI Task entity to use. Leave empty to use the preferred AI Task entity|
|Instructions for the AI|Text|How the AI should interpret the request. `{{ trigger.sentence }}` is the request, `{{ default_list_name }}` the default list|
|Conversation trigger sentences|Text|The sentences which trigger the automation|
|Error response|Text|Used when the AI Task fails, or the items could not be added|

## What the AI determines

|Field|Description|
|---|---|
|`items`|The items to add. "Milk and eggs" are two items|
|`list`|The list, a select with the ids of your lists, so only existing lists can be returned. The AI also gets the names of the lists and whether they support due dates. When no list is mentioned the default list is used|
|`due_date`, `due_time`|Only when mentioned. When only a time is mentioned, today (or tomorrow when that time has passed) is used. Morning is 09:00, afternoon 14:00, evening 19:00 and tonight 20:00|
|`description`|Only when the user gives extra details or a note, and only set when the list supports descriptions|

After adding, the blueprint checks that the items are on the list. The confirmation of the AI is only used when that's the case, otherwise the error response is given.

## Lists without due dates

Not every todo list supports due dates. For example the Shopping list integration doesn't support them at all, and some integrations only support a date without a time. The blueprint checks what the list supports: it sets a date and time when possible, otherwise only the date, and when the list doesn't support due dates the item is added without one. The AI knows which lists support due dates, so it can mention that in the confirmation.

## Translation

### Dutch

```yaml
trigger:
  - "(zet|doe) {item} op [de|mijn] {list}"
  - "voeg {item} toe aan [de|mijn] {list}"
error_response: Sorry, het is niet gelukt om dat op je lijst te zetten
```

## Examples

These are real responses from testing:

- *Add renew passport and book dentist to the testerdetest list, due next friday at half past three in the afternoon*

  *I have added renew passport and book dentist to your testerdetest list, due next friday at half past three in the afternoon.* (both items were added, due Friday 15:30)

- *Zet melk op de boodschappenlijst voor morgen*

  *Ik heb melk toegevoegd aan je boodschappenlijst, maar ik kon de datum van morgen niet opslaan omdat die lijst geen vervaldata ondersteunt.*
