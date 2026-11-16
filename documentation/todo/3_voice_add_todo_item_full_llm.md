<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# Add ToDo item with due date - Full LLM

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Ftodo%2F3_voice_add_todo_item_full_llm.yaml)

Using this blueprint you can create a script which an LLM can use to add one or more items to a todo list, with a due date and time. The LLM only passes the request in a single `query` field. An [AI Task](https://www.home-assistant.io/integrations/ai_task/) (`ai_task.generate_data`) with structured output determines the items, the list, the due date and time and an optional description. The items are added, and the result is returned to the LLM.

## Requirements

* Home Assistant 2026.11 or newer (the blueprint uses `entity_name` for the list names)
* An LLM conversation agent in your voice pipeline which is allowed to control your house (so it can call the script)
* An AI Task entity. This can be the same integration as your conversation agent, but you can also use a smaller and faster model

## How to use the blueprint

1. Import the blueprint using the button above
2. Create a script using the blueprint
3. Select the default list, and optionally the lists which can be used and the AI Task entity
4. Give the script a clear description (example below)
5. Save the script, and expose it to Assist

### Example for script description:

> Adds one or more items to a todo list, with an optional due date and time. Use this tool whenever the user wants to add something to a list, especially when a due date or time is mentioned. Pass the complete request of the user in the query field, in the words of the user.

The built-in tool to add items can't set a due date. When both are available, the description above makes the LLM prefer this script.

## Blueprint settings

|Setting|Type|Description|
|---|---|---|
|Todo lists|Entities|The lists items can be added to. Leave empty to allow all todo lists|
|Default list|Entity|The list used when the request doesn't mention a list|
|AI Task entity|Entity|The AI Task entity to use. Leave empty to use the preferred AI Task entity|
|Instructions for the AI|Text|How the AI Task should interpret the request. `{{ query }}` is the request, `{{ default_list_name }}` the default list|
|Query field description|Text|The description of the `query` field, which tells the LLM what to pass to the script|

## What the AI determines

|Field|Description|
|---|---|
|`items`|The items to add. "Milk and eggs" are two items|
|`list`|The list, a select with the ids of your lists, so only existing lists can be returned. The AI also gets the names of the lists and whether they support due dates. When no list is mentioned the default list is used|
|`due_date`, `due_time`|Only when mentioned. When only a time is mentioned, today (or tomorrow when that time has passed) is used. Morning is 09:00, afternoon 14:00, evening 19:00 and tonight 20:00|
|`description`|Only when the user gives extra details or a note, and only set when the list supports descriptions|

## What the LLM receives

|Key|Description|
|---|---|
|`success`|`true` when all items were added|
|`list`|The name of the list|
|`added`|The items which were added|
|`not_added`|Only when not all items could be added|
|`due`|The due date or date and time which was set|
|`note`|When the list doesn't support due dates (or only a date), so the LLM can tell you|
|`error`|When something went wrong, for example the AI Task failed|

## Lists without due dates

Not every todo list supports due dates. For example the Shopping list integration doesn't support them at all, and some integrations only support a date without a time. The blueprint checks what the list supports: it sets a date and time when possible, otherwise only the date, and when the list doesn't support due dates the item is added without one, and the LLM gets a `note` about it.

## Examples

These are real results from testing:

- *Voeg auto wassen toe aan de testerdetest lijst voor zaterdagochtend, met de notitie: niet vergeten de velgen*

  `{"success": true, "list": "Testerdetest", "added": ["auto wassen"], "due": "2026-11-21T09:00:00+01:00"}`, with the description "niet vergeten de velgen"

- *Add water the plants at 9 pm*

  `{"success": true, "list": "Testerdetest", "added": ["water the plants"], "due": "2026-11-16T21:00:00+01:00"}`
