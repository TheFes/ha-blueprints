<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# Get ToDo items - Full LLM

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Ftodo%2F3_voice_get_todo_entries_full_llm.yaml)

Using this blueprint you can create a script which an LLM can use to list todo items, or answer questions about them.

> [!TIP]
> There is also a [version 2](3_voice_get_todo_entries_full_llm_v2.md) of this script, with a single `query` field and an AI Task to select the list.

## Limitations of this script

* You need to have an LLM integration set up and use it in your voice pipeline
* The LLM integration needs access to your house (Assist), otherwise it can't access the script as a tool

## How to use the blueprint

1. Import the blueprint using the button above
2. Create a script using the blueprint
3. (optionally) adjust the prompt which is used to instruct the LLM how to use the script field
4. Make sure to provide a clear description for the script (example below)
5. Save the script, and expose it to Assist

### Example for script description:

> Fetch todo items from my todo lists. Note that the shopping list is also considered a todo list.

## How the lists are found

The LLM passes one or more lists in the `todo_entities` field. Each one is matched (not case sensitive) on the entity id or the name of the list, first completely, then on a part of the name. When no list matches, the LLM gets an error with the available lists, so it can try again.

Only open items are returned, unless the LLM sets `include_completed` to `true` (when you ask for completed items).

### Usage

You can request the todo items in any way you can think of, using any language. Unless set otherwise in the LLM configuration, the response will be in the same language as the command.

#### Examples

```
What's on the shopping list?
Are there carrots on the shopping list?
Which todo items are planned for today? (only in case the todo items have due dates)
```
