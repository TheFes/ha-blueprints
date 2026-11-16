<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# Get ToDo items - Full LLM - version 2

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Ftodo%2F3_voice_get_todo_entries_full_llm_v2.yaml)

Version 2 of the [Full LLM script](3_voice_get_todo_entries_full_llm.md). The LLM in your voice pipeline only passes the question in a single `query` field. An [AI Task](https://www.home-assistant.io/integrations/ai_task/) (`ai_task.generate_data`) with structured output selects the lists (it can only return lists which exist) and whether open or completed items are requested. Home Assistant gets the items, and the result is returned to the LLM, which answers the question.

It is added next to version 1, because the fields of the script changed. Version 1 keeps working.

## Requirements

* Home Assistant 2026.11 or newer (the blueprint uses `entity_name` for the list names)
* An LLM conversation agent in your voice pipeline which is allowed to control your house (so it can call the script)
* An AI Task entity. This can be the same integration as your conversation agent, but you can also use a smaller and faster model

## How to use the blueprint

1. Import the blueprint using the button above
2. Create a script using the blueprint
3. Optionally select the todo lists which can be used, a default list (for example your shopping list) and the AI Task entity
4. Give the script a clear description (example below)
5. Save the script, and expose it to Assist

### Example for script description:

> Gets the items on my todo lists, including the shopping list. Use this tool for every question about what is on a list. Pass the complete question of the user in the query field, in the words of the user.

## Blueprint settings

|Setting|Type|Description|
|---|---|---|
|Todo lists|Entities|The todo lists which can be used. Leave empty to use all todo lists|
|Default list|Entity|The list used when the question doesn't mention a list (for example the shopping list). Leave empty to use all lists|
|AI Task entity|Entity|The AI Task entity to use. Leave empty to use the preferred AI Task entity|
|Instructions to determine the lists|Text|How the AI Task should determine the lists. `{{ query }}` is the question, `{{ default_list_name }}` the default list|
|Query field description|Text|The description of the `query` field, which tells the LLM what to pass to the script|

## What the LLM receives

The requested status, the current time, the number of items, and per list: the name of the list, the number of items and the items themselves (at most 100 per list). Each item has:

|Key|Description|
|---|---|
|`summary`|The item (line breaks and double spaces removed)|
|`status`|Only when both open and completed items are requested|
|`due`, `due_day`|The due date, and the day in words, for example `Tuesday 18 February 2025`|
|`overdue`|Only present (and `true`) for open items which are past their due date|
|`description`|When set. Cut off after 200 characters|

Only open items are sent, unless the question is about completed items.

When something goes wrong (the AI Task fails, or the items can't be retrieved) the LLM receives an `error`.

## What is different compared to version 1

|Topic|Version 1|Version 2|
|---|---|---|
|Fields|`todo_entities` (and `include_completed`), the LLM has to know the entity ids or exact names of your lists|One field: `query`. The LLM only passes the question|
|Choosing the list|By the LLM, the names have to match a list|By an AI Task, which can only select lists which exist. A default list is used when no list is mentioned|
|Items|As returned by `todo.get_items`, including the uid|Per list with the list name, cleaned up, with the due day in words and `overdue` for open items past their due date|

### Removed and renamed inputs

|Version 1|Version 2|
|---|---|
|`todo_entities_prompt`|`llm_instructions` (for the AI Task) and `query_prompt` (the description of the `query` field)|
|-|New: `todo_entities`, `default_list`, `ai_task_entity`|

### Switching from version 1

Create a new script from the version 2 blueprint (or change the blueprint of your existing script), give it the description above, and make sure only one of them is exposed to Assist.
